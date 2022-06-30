# ProxyJump

Sometimes it is only possible to access a remote server using ssh by [jumping hosts](Jumping-hosts.md). Starting from OpenSSH 7.3, released August 2016, the easiest way to pass through one or more jump hosts is with the ProxyJump directive.

![SSH infra](../../_static/images/infra.png)

The main method here is to use an SSH connection to forward the SSH protocol through one or more jump hosts, using the ProxyJump directive, to an SSH server running on a destination host (this method cannot be used if an intermediate host denies port forwarding). This is considered the most secure method because encryption is end-to-end.

    $ ssh -J user1@jumphost.whatever.org:22 anotherusername@XXX.XXX.XXX.XXX

And can be chained:

    $ ssh -J user1@jumphost1.whatever.org:22,user2@jumphost2.whatever.org:2222 anotherusername@XXX.XXX.XXX.XXX

## Configuration

    Host server2
    HostName XXX.XXX.XXX.XXX
    ProxyJump user1@jumphost1.whatever.org:22
    User anotherusername

    Host server3
    HostName XXX.XXX.XXX.XXX
    ProxyJump user1@jumphost1.whatever.org:22,user2@jumphost2.whatever.org:2222
    User anotherusername
