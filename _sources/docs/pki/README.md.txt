# PKI 

Public key infrastructure (PKI) is the umbrella term for everything needed in order to issue, distribute, store, use, verify, revoke, and otherwise manage and interact with certificates and keys. It’s an intentionally vague term, like “database infrastructure”. Certificates are the building blocks of most PKIs, and certificate authorities are the foundation.

  * [Problems](Problems.md)
  * [Internal PKI](Internal-PKI.md)
  * [Pluggable Authentication Modules (PAM)](PAM.md)
  * [Let's Encrypt](Lets-Encrypt.md)
  * [TLS/SSL](TLS-SSL.md)

## Notes

* Many simple PKIs don’t even use certificates. When editing `~/.ssh/authorized_keys` you’re configuring a simple certificate-less form of PKI that SSH uses to bind public keys to names in flat files.
* Because the PKI system is asymmetric, users need access to a public key and recipients of a message need a private key to decrypt the information. 
* X.509 is a complex standard and complex technology is not easy to deploy on a large-scale. Use a specific profile of X.509 and validate it in a narrow set of implementations.
* Web PKI is mostly defined by RFC 5280 and refined by the CA/Browser Forum. It's hierarchical, based on trusting authorities, and for some such authorities a big cash cow.
* Web PKI being a money-spinner, we also have an alternative. Let’s Encrypt is a free, automated, and open Certificate Authority. 
* Lost keys are also an issue for individuals that rely on encrypted data. Lost keys restrict access to messages, and these messages are unavailable forever unless the cryptographic algorithm is weak and can be cracked.
* Enter the Internet of Things (IoT), the Internet of Everything (IoE) and the Industrial Internet: all the devices that want confidentiality, integrity and authentication related to their identity and that of others too: implanted medical devices, robots on the plant floor, smart light bulbs, ...
* We have a veritable explosion of checks and balances, not to mention issuances, revocations and expirations ... problems, old ones not solved and new ones.
* SSL and PKI infrastructures are essential components in client device security, because they are used to bootstrap into higher-level security services such as software updates and digital signature services. Their vulnerability puts other security services at risk.
* Internal PKI is PKI we run ourselves, for our own stuff: infrastructures with services, containers, and VMs; applications; endpoints like laptops and phones; and any other code or device we want to identify. 
* Use only the minimal set of features in certificates and PKI; That is to use the binding of an identifier and name to a key pair and understand whether the certificate may sign other certificates or not. No additional attributes should be relied on, no complex path validations allowed.
* SSL refers to Secure Socket Layer. It is a protocol for providing security to connections between a server and a client and uses cryptography and hashing to provide confidentiality, integrity, and endpoint authentication to connections between a server and a client. It has a lot of known security bugs of its own.
* TLS refers to Transport Layer Security, and is the successor of SSL, which includes bug fixes and improvements over SSL. SSL came up to versions 3.0 and after that the name was changed to TLS. Hence, often TLS/SSL is used.
