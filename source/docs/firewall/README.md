# Firewalls

Even if services themselves implement security features or are restricted to the interfaces you'd like them to run on, a firewall is still an essential part of server configuration. 

* [IPTables](IPTables.md)
* [NFTables](NFTables.md)
* [FirewallD](FirewallD.md)
* [FireHOL](FireHOL.md)
* [UFW](UFW.md)
* [Packet filtering with I/O Control](PF.md)
* [Fail2ban](Fail2ban.md)
* [SSHguard](SSHguard.md)
* [WAF](WAF.md)
* [Blacklists](Blacklists.md)
* [Port spoofing](Port-spoofing.md)

## Notes

* `Deny All` and then add exceptions to restrict access to everything except the specific services you need to remain open.
* Avoid using `ANY` in `Allow` rules. A rule where the service field is `ANY` can open up 65,535 attack vectors, I mean, TCP ports. 
* Document all rules and use comments for relevant information (purpose, service, users/servers/devices affected, permanent or temporary, name of person that added it) 
* Review proposed and changed firewall rules before implementation
* Consider WAF. Mind that it requires a lot of support work because the defaults are all very strict. Users can not even make linux documentation pages in a dokuwiki without getting thrown out.
