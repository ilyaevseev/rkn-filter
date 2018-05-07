## Scripts

* [rkn-fetch](rkn-fetch) -- check, get or update registry from RKN portal
* [rkn-parse](rkn-parse) -- read registry XML, produce lists of IPaddrs, IPnets, URLs, domains and domain-masks
* [rkn-filter](rkn-filter) -- read lists of URLs, domains and domain-masks, skip overlapped settings, produce Squid configs of URLs and domains.
* [rkn-filter2](rkn-filter2) -- read lists of IP-addrs and IP-nets, produce Squid config without overlappings.
* [ipset-sync](ipset-sync) -- loads IPaddrs and IPnets into firewall
* [iptables-init](iptables-init) -- initializes firewall rules with empty ipsets
* [RUN-ALL-STEPS](RUN-ALL-STEPS) -- runs all scripts above in the right order

## Software requirements

* squid (built with SSL support)
* unzip
* ls4sweep
* iptables-services
* ipset-service

## Perl requirements

* MIME::Base64
* SOAP::Lite
* Net::LibIDN
* Net::CIDR::Lite
* XML::Simple
* URI::Encode

## rkn-fetch

* Usage: `rkn-fetch key-file signature-file command [command args]`
* Commands: info, update, force
* Args for "info": filepath of last downloaded registry (optional)
* Args for "update": target filepath (required), filepath of last downloaded registry (optional, by default = target filepath)
* Args for "force": target filepath (required)

## rkn-parse

* Usage: `rkn-parse dump.xml urlfile domfile dommask_file ipfile netfile`
* dump.xml = input file extracted from registry
* urlfile = output listing of URLs
* domfile = output listing of domains
* dommask = output listing of domain masks
* ipfile = output listing of IPaddrs
* netfile = output listing of IPnets

## rkn-filter

* Usage: `rkn-filter domfile dommask_file urlfile dest-domains dest-urls dest-ports`
* domfile: input list of domains, produced by rkn-parse
* dommask: input list of domain masks, produced by rkn-parse
* urlfile: input list of URLs, produced by rkn-parse
* dest-domains: output domain list in Squid format, without overlapped items
* dest-urls: output URLs list is Squid format, without overlapped items
* dest-ports: output list of ports used in URLs, including 80, 443 and non-standard ports

## rkn-filter2

* Usage: `rkn-filter2 ipfile netfile > aclfile`
* ipfile = input listing of IPaddrs, produced by rkn-parse
* netfile = input listing of IPnets, produced by rkn-parse
* aclfile = output listing in Squid format, without overlapped items

## Dirs

* /home/rkn
* /home/rkn/bin -- should be cloned from this repo
* /home/rkn/keys -- contains request.xml (key) and request.xml.p7s (signature) used by rkn-fetch
* /home/rkn/logs
* /home/rkn/download -- stores actual registry and temporary files
* /home/rkn/old -- stores archive of old registries

## Cron

* `MAILTO=admins`
* `*/30 * * * * root /home/rkn/bin/RUN-ALL-STEPS`
* `30  23 * * * root /home/rkn/bin/delete-old-files 2>&1 | mail -E -s "Delete-old-files failed on $(hostname -f)" admins`
