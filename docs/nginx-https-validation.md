# Nginx HTTPS Validation

## Objective

The objective of this step was to configure Nginx with the certificate issued by the internal Microsoft CA and validate trusted HTTPS access from a domain-joined Windows client.

## Nginx Configuration

The existing HTTP site was updated to redirect HTTP traffic to HTTPS.

The HTTPS virtual host listens on port 443 and uses the certificate issued by the internal CA.

## Nginx Site Configuration

File:

    /etc/nginx/sites-available/web01

Configuration summary:

    server {
        listen 80;
        server_name web01.homelab.local web01 10.10.30.10;

        return 301 https://web01.homelab.local$request_uri;
    }

    server {
        listen 443 ssl;
        server_name web01.homelab.local web01 10.10.30.10;

        root /var/www/web01;
        index index.html;

        ssl_certificate /etc/nginx/ssl/web01.homelab.local.crt;
        ssl_certificate_key /etc/nginx/ssl/web01.homelab.local.key;

        access_log /var/log/nginx/web01_access.log;
        error_log /var/log/nginx/web01_error.log;

        location / {
            try_files $uri $uri/ =404;
        }
    }

## Nginx Validation

The Nginx configuration was tested before reload.

Command used:

    nginx -t

The service was then reloaded.

Command used:

    systemctl reload nginx

Listening ports were verified.

Command used:

    ss -lntp | grep -E ':80|:443'

Expected result:

| Port | Service |
|---:|---|
| 80 | Nginx HTTP redirect |
| 443 | Nginx HTTPS |

## Local Linux Validation

HTTP was verified to redirect to HTTPS.

Command used:

    curl -I http://web01.homelab.local

Expected result:

    HTTP/1.1 301 Moved Permanently
    Location: https://web01.homelab.local/

HTTPS was verified locally.

Command used:

    curl -k -I https://web01.homelab.local

Expected result:

    HTTP/1.1 200 OK

## Windows Client Validation

From the domain-joined Windows client, DNS and TCP connectivity were validated.

Commands used:

    nslookup web01.homelab.local
    Test-NetConnection web01.homelab.local -Port 443

Expected result:

| Test | Expected result |
|---|---|
| DNS resolution | web01.homelab.local resolves to 10.10.30.10 |
| TCP 443 | TcpTestSucceeded = True |
| Browser HTTPS | Trusted certificate, no warning |

## Result

The Windows client trusted the HTTPS certificate because the certificate chain ended at the internal Enterprise Root CA published through Active Directory.
