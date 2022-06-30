# SSH

SSH (Secure Shell) is used for managing networks, operating systems, and configurations. It is also used inside many file transfer tools and configuration management tools. Functionally SSH keys resemble passwords. SSH keys are a pair of cryptographic keys using a public key cryptosystem that can be used to authenticate to an SSH server as an alternative to password-based logins. A private and public key pair are created prior to authentication. The private key is kept secret and secure by the user, while the public key can be shared with anyone.

The advantage SSH authentication has over traditional password authentication is that you can be authenticated by the server without having to send your password over the network. Anyone eavesdropping on unencrypted connections will not be able to intercept and crack the password. It also allows for disabling password-based authentication.

* [Installing SSH](Installing-SSH.md)
* [Harden ssh server](Harden-ssh-server.md)
* [Key management](Key-management.md)
* [Jumping hosts](Jumping-hosts.md)
* [ForwardAgent](ForwardAgent.md)
* [ProxyCommand](ProxyCommand.md)
* [ProxyJump](ProxyJump.md)
* [Authentication error](Authentication-error.md)
* [Keeping current](Keeping-current.md)

## Notes

  * Install and configure ssh keys and make your servers require them to minimise the risk posed by adversaries.  
  * Harden the ssh server (also harden default service settings in web server and other services)
  * Do not leave passphrase empty - An adversary who gets hold of your private key can otherwise connect to the hosts where you put you public key.
  * Use a separate key per client you ssh from to the same server. If one key is compromised the damage is more limited and easier to clean up.
  * Limit the number of clients you ssh from. If a client is compromised an adversary has your key, they can install a binary to get your passwords/passphrases.
  * If any, disable the ssh-server on the client machine you ssh from. Limit the attack surface.
  * Don't make jump host chains too long. If one client in the chain is compromised, an adversary can use that to compromise the rest of the clients down the chain. 
  * Don't use Agent Forwarding. While being connected to the first host, anyone with sufficient permission on the first host will be able to use that socket to connect to and use that local ssh-agent. A rogue root or evil admin with root access can eavesdrop on the ongoing session and impersonate the user for authentication to other servers during the time that user is connected to that server. Instead, use ProxyJump (it uses E2EE) or, if that is not possible, use ProxyCommand.
  * In some countries, proving that you connected to a particular server is enough to be prosecuted, but SSH doesn't provide a native way to obfuscate to whom it connects. For that, SSH with Tor ([[en:security:computer:server:tor:proxy|Tor proxy]]) can be used.
  * Don't ignore authentication errors. This warning is protecting you. The remote host key is different than the client knows. Make sure the key really did change. An adversary may be spoofing the host and continuing may give the adversary the information they seek to attack your server. Another reason to not use passwords. 
