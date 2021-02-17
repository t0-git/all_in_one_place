## Finding sub domains easily

https://twitter.com/TobiunddasMoe/status/1275130179870961664

```
#!/bin/bash
# $1 => example.domain

amass enum --passive -d $1 -o domains_$1
assetfinder --subs-only $1 | tee -a domains_$1

subfinder -d $1 -o domains_subfinder_$1
cat domains_subfinder_$1 | tee -a domains_$1

sort -u domains_$1 -o domains_$1
cat domains_$1 | filter-resolved | tee -a domains_$1.txt
```

---

## Domain related via favicon hash

```
cat my_targets.txt | xargs -I %% bash -c 'echo "http://%%/favicon.ico"' > targets.txt
python3 favihash.py -f https://target/favicon.ico -t targets.txt -s
```

---

## Quickly find with rapiddns

Add this to bash_profile

```
rapiddns(){
curl -s "https://rapiddns.io/subdomain/$1?full=1" \
 | grep -oP '_blank">\K[^<]*' \
 | grep -v http \
 | sort -u
}
```

- Then `rapiddns target.com

---

## Using alienvault

```
curl -s "https://otx.alienvault.com/api/v1/indicators/domain/<TARGET>/url_list?limit=100&page=1" | jq -r '.url_list[].url'
```

## Find subdomains using ASNs with amass

```
amass intel -org <organization> -max-dns-queries 2500 | awk -F, '{print $1}' ORS=',' | sed 's/,$//' | xargs -P3 -I@ -d ',' amass intel -asn @ -max-dns-queries 2500
```

---


