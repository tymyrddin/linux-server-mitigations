# ProxyCommand

Sometimes it is only possible to access a remote server using ssh by[first logging in to another server (or firewall/jump host)](Jumping-hosts.md). ProxyCommand is an alternative for that is supposedly more secure than [SSH agent forwarding](ForwardAgent.md).

![SSH infra](../../_static/images/infra.png)

It is possible to connect via an intermediate machine using a SOCKS proxy. SOCKS4 and SOCKS5 proxies are both supported by OpenSSH. SOCKS5 allows for transparent traversal of a firewall or other application barrier by strong authentication with the help of GSS-API. Dynamic application-level port forwarding allows the outgoing port to be allocated on the fly thereby creating a proxy at the TCP session level.

Using the ProxyCommand option to invoke netcat as the last in the chain is a variation of this for very old clients. The SSH protocol is forwarded by ''nc'' (netcat) instead of ''ssh''. Attention must also be paid to whether or not the username changes from host to host in the chain of SSH connections. The somewhat outdated netcat method does not allow a change of username. Other methods do. 

    ssh -o ProxyCommand='ssh user@jumphost nc backendserver 22' user@backendserver

The ''nc'' command sets and establish a TCP pipe between jumphost (or firewall) and backendserver.

Note: **It is not possible to use both the ProxyJump and ProxyCommand directives in the same host configuration. The first one found is used and then the other blocked.**  

## Configuration

Create or open `~/.ssh/config`

    $ vi ~/.ssh/config

Append the below entry (replacing ''XXX.XXX.XXX.XXX'' with actual server domain name or IP and ''user'' with actual user):

    Host jumphost
    HostName XXX.XXX.XXX.XXX
    User user
    ProxyCommand ssh user@backendserver nc %h %p
