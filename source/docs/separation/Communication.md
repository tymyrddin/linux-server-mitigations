# Communication

Isolate communication between servers in the same account or team within the same region. Only services that are meant to be accessible by clients on the public internet need to be exposed on the public network.

## Making connections
* With a VPN a private network can be mapped that only selected servers can see. Other applications can be configured to pass their traffic over the virtual interface that the VPN software exposes. 
* With some server hosting services, Docker allows for setting up encrypted channels between containers, even when on other servers (within the local network).
* SSH tunnels are useful for allowing outside access to internal network resources and a great way to protect against connections being sniffed by adversaries.
* Sysadmins will need a way to get in if and when an ssh server fails. A minimal VNC with X.509 that combines standard VNC authentication with GNUTLS encryption and server identification can be set up. 
* In some countries, proving that you connected to a particular server is enough to be prosecuted (or just arrested and questioned), but SSH doesn't provide a native way to obfuscate to whom it connects. For that, a Tor proxy server can be set up and clients can SSH over the Tor Onion Service. 

## Future considerations
* Each server in an infrastructure can be configured to trust a centralised certificate authority. Any certificate that the authority signs is then implicitly trusted. If all applications and protocols support TLS/SSL encryption, the system can be encrypted without the overhead of a VPN tunnel (which also often uses SSL internally), at the expense of the non-trivial effort of configuring a certificate authority and setting up the rest of the public key infrastructure and managing the certificates (creating, signing, or revoking). Either way will probably work well until PKI (a system that is designed to create, manage, and validate certificates for identifying individuals and encrypting communication) is worth the extra effort (and associated costs).
* SSL or TLS certificates can be used to authenticate different entities to one another. After authentication, they can also be used to establish encrypted communication. This can prevent MITM attacks where an attacker imitates a server in an infrastructure to intercept traffic. And, [E2EE is not sacred](https://tymyrddin.github.io/e2ee-threat-model). We will need to protect from SSL Spoofing.

