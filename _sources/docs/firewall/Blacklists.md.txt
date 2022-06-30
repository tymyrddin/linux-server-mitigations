# Blacklists

To actively blacklist bad actors is a cost-effective defense. Real-time blacklists are services that publish the IP addresses of computers which send spam, or exhibit a particular characteristic, such as using a dynamically assigned address: OpenBL, SpamHaus, Dshield, MalcOde, Myip.ms, Squidblacklist, Malwaredomainlist.com, Blocklist.de, Zeustracker and many others offer blacklisted IP address lists for daily downloads.

## Example script
The idea here is to automatically integrate and update all these blacklisted IP databases in a router and create blocking rules in the firewall. This needs trying and testing!

Create a deny folder in the `/etc/nginx` folder

    # mkdir /etc/nginx/deny

Create a script to download and format the configuration files for Nginx. 

    #!/bin/sh
    saveto=/etc/nginx/deny
    now=$(date);

    echo `date` > $saveto/dshield-deny.conf
    wget -O - http://feeds.dshield.org/block.txt | awk --posix '/^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.0\t/ { print "deny " $1 "/24; # comment=DShield"}' >> $saveto/dshield-deny.conf

    ...

    echo `date` > $saveto/example-deny.conf
    wget -O - http://example.com/hostslist/ip.txt | awk --posix '/^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}/ sub("\r$", "") { print "deny " $1 "; # comment=examplelist"}' >> $saveto/example-deny.conf


Edit `/etc/nginx/nginx.conf` and append:

    include /etc/nginx/deny/*.conf;

Restart nginx after each update of the configuration files:

    # systemctl nginx restart

If and when it works, make a crontab entry to run it regularly. Add `rm $saveto/*` before the downloads to clear current config files and `systemctl nginx restart` at the end to restart Nginx.

## IP Sets

Tools like [IP sets](http://ipset.netfilter.org) can be used with [iptables] to create large black-lists or white-lists than can easily block or allow tens of thousands of IP's with almost no impact on performance. 

* https://iplists.firehol.org/files/tor_exits_30d.ipset
* https://iplists.firehol.org/files/cybercrime.ipset

```
#!/bin/sh
saveto=/etc/nginx/deny
now=$(date);

echo `date` > $saveto/tor-deny.conf
wget -O - https://iplists.firehol.org/files/tor_exits_30d.ipset | awk --posix '/^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}/ { print "deny " $1 "; # comment=tor"}' >> $saveto/tor-deny.conf

echo `date` > $saveto/cybercrime-deny.conf
wget -O - https://iplists.firehol.org/files/cybercrime.ipset | awk --posix '/^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}/ { print "deny " $1 "; # comment=cybercrime"}' >> $saveto/cybercrime-deny.conf
```

