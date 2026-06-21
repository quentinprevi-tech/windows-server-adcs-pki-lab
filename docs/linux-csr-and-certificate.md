# Linux CSR and Certificate Issuance

## Objective

The objective of this step was to generate a certificate signing request on the Linux/Nginx web server and issue a certificate from the Microsoft AD CS Certificate Authority.

## Web Server

| Setting | Value |
|---|---|
| Hostname | web01 |
| FQDN | web01.homelab.local |
| IP address | 10.10.30.10 |
| Operating system | Debian |
| Web server | Nginx |

## CSR Configuration

The CSR was generated directly on the Linux server.

The OpenSSL configuration included both the common name and Subject Alternative Names.

Requested names:

| Type | Value |
|---|---|
| DNS | web01.homelab.local |
| DNS | web01 |
| IP | 10.10.30.10 |

## OpenSSL Configuration File

File used:

    /etc/nginx/ssl/web01-openssl.cnf

Configuration content:

    [ req ]
    default_bits       = 2048
    prompt             = no
    default_md         = sha256
    distinguished_name = dn
    req_extensions     = req_ext

    [ dn ]
    CN = web01.homelab.local

    [ req_ext ]
    subjectAltName = @alt_names

    [ alt_names ]
    DNS.1 = web01.homelab.local
    DNS.2 = web01
    IP.1 = 10.10.30.10

## Private Key and CSR Generation

Command used on web01:

    openssl req -new -nodes \
      -out web01.homelab.local.csr \
      -newkey rsa:2048 \
      -keyout web01.homelab.local.key \
      -config web01-openssl.cnf

## CSR Validation

The CSR was validated before being submitted to the CA.

Command used:

    openssl req -in web01.homelab.local.csr -noout -text

The CSR contained the expected Subject Alternative Names:

    DNS:web01.homelab.local
    DNS:web01
    IP Address:10.10.30.10

## Certificate Issuance

The CSR was copied to SRV-CA01 and submitted to the Microsoft CA using certreq.

Command used on SRV-CA01:

    certreq -submit -attrib "CertificateTemplate:HomelabWebServer" C:\web01.homelab.local.csr C:\web01.homelab.local.cer

The certificate was issued by:

    HOMELAB-SRV-CA01-CA

## Certificate Validation on Linux

After copying the issued certificate back to web01, it was validated with OpenSSL.

Command used:

    openssl x509 -in /etc/nginx/ssl/web01.homelab.local.crt -noout -subject -issuer -dates

Expected result:

| Field | Value |
|---|---|
| Subject | CN=web01.homelab.local |
| Issuer | HOMELAB-SRV-CA01-CA |
| Validity | 2 years |

## File Permissions

The private key was restricted to root.

Commands used:

    chown root:root /etc/nginx/ssl/web01.homelab.local.*
    chmod 600 /etc/nginx/ssl/web01.homelab.local.key
    chmod 644 /etc/nginx/ssl/web01.homelab.local.crt

## Security Note

The private key must never be committed to GitHub.

Only documentation, sanitized screenshots and validation outputs should be published.
