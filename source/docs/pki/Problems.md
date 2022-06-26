# Problems

PKI is failing by trying to be all things to all people and things. PKIs have experienced a hype and many companies and organizations announced to provide certification services to the general public. Only a few of these have succeeded and actually provide certification services that can be taken seriously.

## X.509 standard

X.509 is a complex standard and X.509 certificates are complex data structures with a lot of fields, many of which are critical and/or non-critical extension fields. Complex technology is not easy to deploy on a large-scale. Since Peter Gutmann from Auckland University published the X.509 Style Guide in 2000, little has improved.

* X.509 certificates are specified with ASN.1 and the resulting data structures are not very meaningful for humans (and hard to parse).
* The X.509 specification is scattered across many standards, drafts and amendments from multiple standard bodies, leading to confusion and numerous profiles that lack interoperability.
* Parts of the standard, even some that are related to key elements, are underspecified. That leads to ambiguity, unknown semantics and interoperability issues, like the country code, date/time formats in validity or path validation.

## Technical

* Public key pairs must be generated in an efficient and secure (centralized or decentralized) way, and require a cryptographically strong pseudo-random bit generator, something which is not always available to end-users.
* Private keys must be securely stored in a personal security environment (and not a vulnerable endpoint), and for [Web PKI](TLS-SSL.md) public CA's certificates must be made publicly available in a certificate repository or directory service. Hierarchical trust models are dangerous, because  they provide a single point of attack. Alternative models (for example the mesh model) are poorly scalable in time and in space.
* Certificate authorities (CA's) have to periodically send out a list of certificates that have been revoked (CRL's), meaning that a user will not only have to get the corresponding certificates from the certification chain, but will also have to check that those certificates are not in the CRL.
* The definition and maintenance of a global name space is not as simple in practice.
* Cross-certification does not work in practice because competition is rule.

## Economical
* The establishment and operation of a top CA requires large investments. One needs protection from adversaries trying to break into the facility, intercepting electromagnetic signals, and exploitation of other weaknesses of the used hardware and software, and one needs qualified personnel able to withstand all of the regulation and bureaucratic stuff.
* The ROI of a top CA is difficult to determine and quantify. Like many other infrastructure components, public key certificates do not provide a specific function in and of themselves, but provides only the means to secure functions and applications of other infrastructures. IOW, business cases for PKI's are extremely hard to make. That part got solved, with some scary stuff happening, web PKI next in the chain, and hosting companies (re)selling certificates.

## Juridical
* If a digital signature is generated according to some digital signature law, then somebody must be made liable if something goes wrong. If that is the Credential service provider (CSP), the insurance protection must be taken into account in a business case.
* As Bruce Schneier pointed out in [Secrets and Lies](https://www.schneier.com/books/secrets_and_lies/), CA's usually limit their liability to the protection of their private key. As the CA plays the role of a trusted third party users of certificates are put at a disadvantage. CA's are like a notary who would only be liable to keep his stamp secure. Nothing changed much there either.
* The owner of a public key certificate cannot repudiate a signature that is generated with the appropriate signing key, which may lead to the situation in which a certificate owner may be held liable and accountable for statements that are digitally signed with the proper key, even if he or she does not know (and does not agree) with the statements. 

## Social
* In the real world, trust is based on two-way relationships and experiences that have been made over time. This can only  insufficiently be modelled with hierarchical certificate models.
* Poor usability is a common feature of many products that employ (public key) cryptography. Secure cryptographic solutions and usability seem mutually exclusive. But then, there is not much focus on end users and UX design in general. What's new?
* The users of public key cryptography are often not aware of the vulnerabilities and pitfalls. 

## Privacy and security

* X.509 uses features from other standards, like DNs from X.500 which are in itself complex and have little value and provides unanticipated attack vectors.
* SSL in the context of web browsers used to be target of several successful attacks, either directly on SSL (e.g. SSL renegotiation bug), the combination of https and http (SSL Strip), URL-forgery, and X.509-based attacks on ASN.1 and DN-parsing.
* Integration of PKI with the surrounding environment (end points, web applications) may allow compromise.
* Should a private key be compromised, an attacker would have access to data intended for the recipient. Attackers that gain access to private keys can also eavesdrop on content intended for a recipient and decrypt data as it’s collected. This is the biggest threat to the PKI system, because compromised keys require new keys to be issued, and old ones revoked.
* There is no easy, fast and effective method to revoke a root certificate. Anything less that a full compromise of the root key will tempt a CA to downplay the incident and revoke only certain certificates. This occurred with the DigiNotar-Hack, because the revocation of the root-CA would have interrupted the operation of many services.
* Revocation services are suspect to DoS-attacks.
* CAs are included in browsers by OS- and browser vendors without a common consistent policy. To be included in the trust store the vendor has to pay a fee. Only Microsoft requires a formal audit. As a result there is a sufficient number of weak and very weak CAs that are included in default certificate stores. In addition there is suspicion in the security community that governments use compelled certificates to tap Internet communication.
*  Clients certificates and their attributes have zero security value, because there are CAs that do not validate the attributes including attributes with arbitrary OIDs (P. Gutmann).
* Some certificate stores contain funny root certificates that should be excluded on any serious security review.

## Alternatives
A variety of alternative approaches, ranging from simple workarounds to designing the application to sidestep PKI's shortcomings, can help solve the problems inherent in the standard X.509 model. For example, in simple public key infrastructure (SPKI) and simple distributed security infrastructure (SDSI), keys are not bound to identities, but to roles or authorisations and names only have to be unique locally.

## Resources

* [PKI interoperability: Still an issue? A solution in the X.509 realm](https://hal.inria.fr/hal-01463660/document), Ahmad Samer Wazan Romain Laborde, François Barrere, Abdelmalek Benzekri, David W Chadwick, 2017
* [Why have public key infrastructures failed so far?](https://pdfs.semanticscholar.org/1879/2962f8150e074ba5373cf1535a87a51e37fd.pdf), Javier Lopez, Rolf Oppliger, Gunther Pernu, 2005
* [PKI: It’s Not Dead, Just Resting](https://www.cs.auckland.ac.nz/~pgut001/pubs/notdead.pdf), Peter Gutmann, 2002
* [The Emperor’s New Clothes:  The Shocking Truth About Digital Signatures and Internet Commerce](https://digitalcommons.law.uw.edu/faculty-articles/167/), Jane K. Winn, 2001
* [X.509 Style Guide, Peter Gutmann](https://www.cs.auckland.ac.nz/~pgut001/pubs/x509guide.txt), 2000
