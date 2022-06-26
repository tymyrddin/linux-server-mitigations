# ForwardAgent

Sometimes it is only possible to access a remote server using ssh by [first logging in to another server (or firewall/jump host)](Jumping-hosts.md). SSH agent forwarding can be used to make access to a server simple. It allows for the use of local SSH keys instead of leaving keys (without passphrases!) sitting on the server.

![SSH infra](../../_static/images/infra.png)

The -A option enables forwarding of the authentication agent connection:

    ssh -A user@server.com 

It forwards the SSH authentication schema to the remote host to allow for the use of SSH on the jump host as if the user was on the local machine. A forwarding socket will be set up so that the SSH client on the first host can connect to the `ssh-agent` on the client machine to perform authentication on its behalf. The key is not forwarded, but the agent, meaning many keys can be added.

## Configuration

Create or open `~/.ssh/config`:

    $ vi ~/.ssh/config

Append the below entry (replacing XXX.XXX.XXX.XXX with actual server domain name or the IP):

    Host jumphost
    HostName XXX.XXX.XXX.XXX
    User user
    ForwardAgent yes

## Security problem

The problem is that while being connected to the first host, anyone with sufficient permission on the first host will be able to use that socket to connect to and use that local ssh-agent. A rogue root or evil admin with root access can eavesdrop on the ongoing session and impersonate the user for authentication to other servers during the time that user is connected to that server.

A mentioned mitigation is using the `-c` option, which will show a confirmation window each time some program wants to use the agent to authenticate somewhere. Recommended is using [ProxyCommand](ProxyCommand.md) instead. Or even better, [ProxyJump](ProxyJump.md).

