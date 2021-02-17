Prepare_vps_oob_and_else_(godaddy domain)

## How to

### Domain

- Register a new domain.
- Add these records :
```
Type	Name		Value			TTL
A		@			<ip>			600	
A		ns1			<ip>			600	
CNAME	www			@				600	
NS		oob			ns1.<domain>	600	
```

### Installation

- Install ufw, apache2, snapd.

```
ufw limit ssh
ufw allow 'Apache Full'
ufw allow 53
ufw allow 953/tcp
```

- Check if snapd is up to date : `snap install core`
- Install certbot with snapd and add it to the path : `sudo snap install --classic certbot && sudo ln -s /snap/bin/certbot /usr/bin/certbot`

### Automaticaly request certificate and renew

- `git clone https://github.com/orthrus/Certbot-Godaddy.git`
- Create an API key on godaddy developper website
- Enter you GoDaddy API and domain information in the `api-settings.sh` file
- Edit `certbot-renew-post-hook.sh` and add `systemctl restart apache2`
- Request a new certificate `certbot-godaddy-request.sh`
- `0 0 1 * * /path/to/certbot-godaddy-renew.sh`
- `chown root:root /path/to/api-settings.sh`
- `chmod 700 /path/to/api-settings.sh`

### Manualy (you'll have to renew between every 60 and 90 days)

`certbot certonly --manual --preferred-challenges=dns --server https://acme-v02.api.letsencrypt.org/directory --agree-tos --manual-public-ip-logging-ok -d "*.<domain>" -d "<domain>" --email <mail>` 

- Copy paste the name (just _acme-challenge) and the value, **don't forget to hit `CTRL-C`** and create a TXT record on your registrar 

- Wait to cactch it with `dig`: `dig _acme-challenge.<yourdomain> TXT`

- Launch the command again, it should work. 

### Apache2

- `a2enmod ssl`
- `a2ensite default-ssl.conf`
- for the ssl part, this part is mandatory : 

```
SSLEngine on
SSLCertificateFile      /etc/letsencrypt/live/<domain>/fullchain.pem
SSLCertificateKeyFile   /etc/letsencrypt/live/<domain>/privkey.pem
```

- Add this too :
```
ServerName <domain>
ServerAlias www.<domain>
```

- If you want to log full URLs, add this in your apache2 conf : `%{Host}i%U%q`

- Enjoy !