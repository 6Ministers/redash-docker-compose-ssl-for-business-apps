# This instruction is not yet working

Installing Redash+Caddy+SSL with docker-compose.

## Quick Installation

**Before starting the instance, direct the domain (subdomain) to the ip of the server where Redash will be installed!**

## 1. Redash Server Requirements
From and more
- 2 CPUs
- 4 RAM 
- 10 Gb 

Run for Ubuntu 22.04

``` bash
sudo apt-get purge needrestart
```

Install docker and docker-compose:

``` bash
curl -OL https://raw.githubusercontent.com/6Ministers/redash-docker-compose-ssl-for-business-apps/master/setup.sh 
```

Activate the script
``` bash
chmod +x setup.sh
```

``` bash
sudo ./setup.sh
```

Navigate to the folder you created `redash`

``` bash
cd /opt/redash
```

Create the `Caddyfile`with the entry listed below


To change the domain in the `Caddyfile` to your own

``` bash
https://redash.your-domain.com:443 {
        reverse_proxy 127.0.0.1:5000
#	tls admin@example.org
	encode zstd gzip
	file_server
	
	# Secure headers, all from .htaccess except Permissions-Policy, STS and X-Powered-By
	header {
		Strict-Transport-Security max-age=31536000
		Permissions-Policy interest-cohort=()
		X-Content-Type-Options nosniff
		X-Frame-Options SAMEORIGIN
		Referrer-Policy no-referrer
		X-XSS-Protection "1; mode=block"
		X-Permitted-Cross-Domain-Policies none
		X-Robots-Tag "noindex, nofollow"
		-X-Powered-By
}

}

```


**Run Redash:**

``` bash
sudo docker-compose up -d
```

Then open `https://redash.domain.com:` to access Redash

## Redash container management

**Run Redash**:

``` bash
sudo docker-compose up -d
```

**Restart**:

``` bash
sudo docker-compose up restart
```

**Restart**:

``` bash
sudo docker-compose down && sudo docker-compose up -d
```

**Stop**:

``` bash
sudo docker-compose down
```

## Documentation

https://github.com/getredash/setup/tree/master

https://github.com/getredash/setup/blob/master/setup.sh

https://github.com/getredash/setup/blob/master/data/docker-compose.yml