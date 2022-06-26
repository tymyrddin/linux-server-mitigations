# Chroot jails

Setting up a `chroot` environment for each component provides some level of isolation, but there are many easy ways of breaking out of a `chroot` environment. Moving components to dedicated machines is better, often easier, and more costly as it requires more (virtual) machines, and securing and maintaining each and every server.

A `chroot` is an operation that changes the apparent root directory for the current running process and their children. It temporarily changes the root directory (which is normally /) to the chroot directory (for example, /var/chroot). As the root directory is the top of the filesystem hierarchy, applications are unable to access directories higher up than the root directory, hence are isolated from the rest of the system. This prevents applications inside the chroot from accessing files and commands elsewhere on the server. This modified environment is called a //chroot jail//.

Changing root is done for performing system maintenance on systems where booting and/or logging in is no longer possible, such as reinstalling a bootloader, rebuilding an initramfs image, upgrading or downgrading packages, resetting a forgotten password, building packages in a clean chroot, etc. 

## Create jail
    
To create a chroot jail:
    # chroot /path/to/new/root command
or
    # chroot /path/to/new/root /path/to/server
or
    # chroot [options] /path/to/new/root /path/to/server

For a chrooted program to successfully start, the chroot directory must be populated with a minimum set of scratch space, configuration files, device nodes and shared libraries. This can make chroot difficult to use as a general sandboxing mechanism. 

## Simple jail with bash and ls

Building a “virtual root” containing every file the commands may need.

Create a directory which will become the root of the command

```
$ mkdir jail
$ cd jail
```

Create the necessary directories

```
$ mkdir -p bin lib64/x86_64-linux-gnu lib/x86_64-linux-gnu
```

Copy location of `ls` and `bash` with `which` (unalias both first if having aliased them)

```
$ cp $(which ls) ./bin/
$ cp $(which bash) ./bin/
```
 
Find dependencies of `bash`

```
$ ldd $(which bash)
linux-vdso.so.1 (0x00007ffc1f71a000)
libtinfo.so.5 => /lib/x86_64-linux-gnu/libtinfo.so.5 (0x00007f46ec9a6000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f46ec7a2000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f46ec403000)
/lib64/ld-linux-x86-64.so.2 (0x00007f46ecbd0000)
```

Create directories

```
$ cp /lib/x86_64-linux-gnu/libtinfo.so.5 lib/x86_64-linux-gnu/
$ cp /lib/x86_64-linux-gnu/libdl.so.2 lib/x86_64-linux-gnu/
$ cp /lib/x86_64-linux-gnu/libc.so.6 lib/x86_64-linux-gnu/
$ cp /lib64/ld-linux-x86-64.so.2 lib64/
```

Find dependencies of `ls`

```
$ ldd $(which ls)
linux-vdso.so.1 (0x00007ffdf15bf000)
libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007fcd43e2a000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fcd43a8b000)
libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007fcd43818000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fcd43614000)
/lib64/ld-linux-x86-64.so.2 (0x00007fcd44273000)
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fcd433f7000)
```

Create directories

```
$ cp /lib/x86_64-linux-gnu/libselinux.so.1 lib/x86_64-linux-gnu/
$ cp /lib/x86_64-linux-gnu/libc.so.6 lib/x86_64-linux-gnu/
$ cp /lib/x86_64-linux-gnu/libpcre.so.3 lib/x86_64-linux-gnu/
$ cp /lib/x86_64-linux-gnu/libdl.so.2 lib/x86_64-linux-gnu/
$ cp /lib64/ld-linux-x86-64.so.2  lib64/
$ cp /lib/x86_64-linux-gnu/libpthread.so.0 lib/x86_64-linux-gnu/ 
```
  
Go up, change the root and path to the shell

```
$  cd ..
$ sudo chroot jail /bin/bash
[sudo] password for nina: 
bash-4.4#
```

Exit the jail with

```
bash-4.4# exit
```

Processes in the jailed shell run as a simple child processes and are a user level process in the host OS and isolated by the namespaces provided by the kernel => isolation and minimal overhead. While the jail is running, check in another terminal with:

```
$ ps aux | grep "chroot"
avahi      852  0.0  0.0  47012    32 ?        S    Jul31   0:00 avahi-daemon: chroot helper
root     27884  0.0  0.0  55476  3680 pts/0    S    15:04   0:00 sudo chroot jail /bin/bash
```

## Chrooting webservers

The above example just demonstrates the basic principles. More (complex) commands can be added to a jail that may require the creation of more directories such as `/proc` and `/dev`. 

Chroot jails are commonly used for Apache and Nginx web servers. 

### Upgrading

Upgrade the chrooted web server, as well as php and related extensions:

- Stop or shutdown web server
- Backup existing chrooted directory to another partition or directory
- Download latest web server. Upgrade web server. 
- Upgrade php and php-mysql extensions
- Upgrade chrooted web server located at `/chroot` or `/jail` or `/whatever-its-called`
- Copy the mysql extension and copy all the php shared modules such as php-imap, php-gd, php-memcache etc.
- Start the web server
- Check all is running as intended

```
# tail -f /var/log/message
# netstat -tulpn
```

### Limitations 

* Some systemd tools such as `hostnamectl`, `localectl` and `timedatectl` can not be used inside a `chroot`, as they require an active //dbus// connection. 
* The file system that will serve as the new root (/) of a chroot must be accessible (decrypted and mounted).  When using `--rbind`, some subdirectories of `dev/` and `sys/` will not be unmountable. Attempting to unmount with `umount -l` in this situation will break the session and requires a reboot. If possible, use `-o bind` instead.

## Security?

The `chroot` mechanism is **NOT** intended to defend against intentional tampering by privileged (root) users. On most systems, //chroot contexts// do not stack properly and chrooted programs with sufficient privileges may perform a second `chroot` to break out. To mitigate the risk of this security weakness, chrooted programs should relinquish root privileges as soon as practical after chrooting. Or use FreeBSD jails instead. FreeBSD takes precautions to prevent the second chroot attack.

On systems that support device nodes on ordinary filesystems with special files, a chrooted root user can still create device nodes and mount the file systems on them. The chroot mechanism is **NOT** intended by itself to be used to block low-level access to system devices by privileged users. It is **NOT** intended to restrict the use of resources like I/O, bandwidth, disk space or CPU time. When a system is **NOT** completely file system-oriented, it leaves potentially disruptive functionality like networking and process control available through the system call interface to a chrooted program. 

## Without root

### fakeroot 
In principle, only the root user can perform a chroot, to prevent users from putting a `setuid` program inside a chroot jail (for example, with a fake `/etc/passwd` and `/etc/shadow` file) that would fool it into a privilege escalation.

* `fakechroot` is a library shim which intercepts the `chroot` call and fakes the results. 
* `fakeroot` runs a command in an environment where it appears to have root privileges for file manipulation.

They can be used in conjunction to simulate a chroot as a regular user. This is useful for allowing users to create archives (tar, ar, .deb etc.) with files in them with root permissions/ownership. Without `fakeroot` one would have to have root privileges to create the constituent files of the archives with the correct permissions and ownership, and then pack them up, or one would have to construct the archives directly, without using the archiver. 

### schroot
Or use the `schroot` wrapper which allows unprivileged users to have access to one or more `chroot` environments. `schroot` handles the `chroot` call as well as dropping privileges inside the jail, setting up `/etc/resolv.conf` and bind mounting resources into the jail (like `home` directories, `/dev`, `/sys`, and `/proc`)

### proot
PRoot may be used to change the apparent root directory and use `mount --bind` without root privileges. This is useful for confining applications to a single directory or running programs built for a different CPU architecture, but it has limitations due to the fact that all files are owned by the user on the host system. PRoot provides a `--root-id` argument that can be used as a workaround for some of these limitations in a similar (albeit more limited) manner to fakeroot. 
