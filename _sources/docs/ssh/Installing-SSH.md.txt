# Installing SSH

## openSSH server 

    # apt-get install openssh-server
    # systemctl enable ssh
    # systemctl start ssh

### Control

On Debian, the default behaviour of an OpenSSH server is that it will start automatically as soon as it is installed. Verify it is running:

    # systemctl status ssh

If it is not running, try to start it with:

    # systemctl start ssh

Stop it with

    # systemctl stop ssh

If you wish to disable OpenSSH server from starting up at boot:

    # systemctl disable ssh

And enable for starting up at boot:

    # systemctl enable ssh

## Filtering

Allow SSH connections in the firewall.

## openSSH client

    § sudo apt-get install openssh-client

* [Generate a key](Key-management.md)
* Punch a hole in the firewall
  * If `ufw` is installed on client, open port (22, oruse the port you need)   
* Copy key to server.
* Connect and login.

