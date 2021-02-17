## API KEY

- Create an alias using 'amass --config <config_file>'
- Create an account and paste the API key for all these feeds : 
```
AlienVault, BinaryEdge, BufferOver, BuiltWith, C99, Censys, Chaos, CIRCL, DNSDB, DNSTable, FacebookCT, GitHub, HackerOne, HackerTarget, NetworksDB, PassiveTotal, RapidDNS, Riddler, SecurityTrails, Shodan, SiteDossier, Spyse, Twitter, Umbrella, URLScan, VirusTotal, WhoisXML, ZETAlytics, Cloudflare
```


# The 5 Holy Subcommands


*   amass intel — Discover targets for enumerations
*   amass enum — Perform enumerations and network mapping
*   amass viz — Visualize enumeration results
*   amass track — Track differences between enumerations
*   amass db — Manipulate the Amass graph database


# Amass Intel

Mostly, it’s good for finding root domains and additional subdomains that belong to a company. It can do this in a number of ways. Firstly, there’s the “reverse whois” method.

## Reverse Whois

This method is invoked using the `-whois` flag. Essentially it takes the details from the specified domain’s whois records, and then tries to find other domains with similar whois records. As an example, I performed this task on `owasp.org` in the screenshot below:

```amass intel -d owasp.org -whois```


This is a great method for discovering root domains that may be owned by an organisation.

## SSL Certificate Grabbing

If you feed IP addresses to Amass and give it the `-active` flag, it pulls the SSL certificate from every IP address within the IP range and then spits back the domain that the SSL cert is associated with. 

```amass intel -active -cidr 173.0.84.0/24```

## Using ASNs

If you want to find the ASN of a company, Amass can do a nice convenient search. For example, if I wanted to find ASN’s associated with Tesla, I could use this:

``` amass intel -org "Tesla"```

Now we can use that ASN number (394161) to get some more domains, like this:

```amass intel -active -asn 394161```

## Putting Amass intel techniques together recursively

Amass works recursively. It will take the results that it gets from one method, and feed it into the other method. It will continue to do this until no new results are returned. So, for example, we can do this:

```
amass intel -asn 394161 -whois -d tesla.com
```

# Amass enum


## Get some subdomains

```
amass enum -d example.com
```

It’s good, but it could be better.

## Get more subdomains

Combining the options will usually lead to better results.

```
amass enum -d example.com -active -cidr 1.2.3.4/24,4.3.2.1/24 -asn 12345
```

Note that you will first need to get the CIDRs and ASNs associated with the organisation using the `intel` methods outlined earlier in this post.

# Tracking

Every scan that you do with amass is automatically stored on the computer that you ran it on. Then, if you run the same scan again, amass will track any changes that have taken place since your last scan. The most obvious way to use this feature is to discover which subdomains have appeared since your last scan. For example, I ran `amass enum -d vimeo.com` back in June. It’s now August, so I ran the same command again.

Now I can run `amass track -d vimeo.com` and it will tell me anything that has changed within the last two months.


Focusing on these subdomains means that I’m focusing on fresher targets, which are (hopefully) more likely to be vulnerable.

# Visualisation

You can use `amass viz` to create awesome-looking graphs of your recon data. To be totally honest, I never use this feature. I’m glad it’s there and I think it’s cool, but as a bug bounty hunter, I find subdomains in a text file to be more valuable than a graph. It does look pretty badass though.

# Amass db

`db` is the subcommand that allows you to view the recon data for every scan that you’ve ever done.

To list all of the details of all of your previous scans, simply run `amass db -show`. If you want to see details of a specific domain, just add the `-d` flag: `amass db -show -d paypal.com`. If you would prefer a nice clean, plain output, you can output the discovered domains/subdomains.


https://danielmiessler.com/study/amass/

https://medium.com/@hakluke/haklukes-guide-to-amass-how-to-use-amass-more-effectively-for-bug-bounties-7c37570b83f7