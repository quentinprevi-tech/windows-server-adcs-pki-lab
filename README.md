# Windows Server AD CS / PKI Lab

## Overview

This project documents a Microsoft Active Directory Certificate Services lab built in a Proxmox homelab.

The goal was to deploy an internal Public Key Infrastructure using Windows Server AD CS, issue a trusted certificate for a Linux/Nginx web server, validate internal HTTPS from a domain-joined Windows client, and monitor the Certificate Authority with Zabbix.

## What this proves

This project demonstrates the ability to deploy and validate an internal Microsoft PKI in an Active Directory environment.

It shows practical understanding of AD CS, certificate trust, certificate templates, certificate signing requests, internal HTTPS validation, Windows domain trust, Linux/Nginx certificate deployment and monitoring of critical certificate services.

## Lab Objectives

The main objectives were to:

- Deploy a dedicated Windows Server Certificate Authority.
- Join the CA server to the Active Directory domain.
- Install and configure Active Directory Certificate Services.
- Configure an Enterprise Root CA.
- Validate that domain clients trust the internal CA.
- Create and publish a Web Server certificate template.
- Generate a CSR on a Linux/Nginx server.
- Issue a certificate from the Microsoft CA.
- Configure Nginx with HTTPS using the issued certificate.
- Validate trusted HTTPS from a Windows domain client.
- Monitor the CA server and CertSvc service in Zabbix.
- Document the architecture, validation steps and security considerations.

## Lab Environment

| Component | Role |
|---|---|
| Proxmox VE | Virtualization platform |
| OPNsense | Firewall and network segmentation |
| SRV-AD01 | Active Directory Domain Controller and DNS |
| SRV-CA01 | AD CS Certificate Authority |
| WIN11 client | Domain-joined validation client |
| web01 | Debian/Nginx web server |
| Zabbix | Monitoring platform |

## Network Layout

| System | IP Address | Role |
|---|---:|---|
| SRV-AD01 | 10.10.20.10 | Active Directory and DNS |
| SRV-CA01 | 10.10.20.90 | Certificate Authority |
| web01 | 10.10.30.10 | Debian/Nginx web server |
| win11-client-lab | 10.10.10.105 | Domain-joined client |
| OPNsense SERVERS gateway | 10.10.20.1 | Firewall gateway |
| OPNsense DMZ gateway | 10.10.30.1 | DMZ gateway |

## PKI Design

This lab uses an Enterprise Root CA integrated with Active Directory.

| Setting | Value |
|---|---|
| CA server | SRV-CA01 |
| CA type | Enterprise Root CA |
| CA name | HOMELAB-SRV-CA01-CA |
| Domain | homelab.local |
| Hash algorithm | SHA256 |
| Key length | 2048-bit RSA |
| CA validity | 10 years |

For a production environment, a two-tier PKI with an offline Root CA and an online Issuing CA would be more appropriate. This lab uses a simplified design for learning and portfolio purposes.

## Certificate Workflow

The certificate workflow used in this lab was:

1. Generate a private key and CSR on the Linux web server.
2. Submit the CSR to the Microsoft CA.
3. Issue the certificate using a custom Web Server template.
4. Install the signed certificate and private key in Nginx.
5. Configure Nginx HTTPS on port 443.
6. Validate the certificate from a domain-joined Windows client.

## HTTPS Validation

The internal web server was configured with a certificate issued by the internal CA.

| Setting | Value |
|---|---|
| HTTPS URL | https://web01.homelab.local |
| Certificate subject | web01.homelab.local |
| Issuer | HOMELAB-SRV-CA01-CA |
| Web server | Nginx |
| Validation client | Windows 11 domain client |

The Windows client trusted the certificate because the Enterprise Root CA was published through Active Directory.

## Monitoring

SRV-CA01 was added to Zabbix using the Windows Zabbix Agent.

The AD CS service was monitored with a dedicated trigger.

| Monitored item | Purpose |
|---|---|
| SRV-CA01 availability | Confirms that the CA server is reachable |
| Windows metrics | CPU, memory, disk and network monitoring |
| CertSvc service state | Detects if Active Directory Certificate Services stops |
| CertSvc trigger | High severity alert if the CA service is not running |

## Screenshots

Screenshots are stored in the `screenshots/` directory.

Recommended screenshots:

| Screenshot | Description |
|---|---|
| `01-enterprise-root-ca-trusted-on-client.png` | Domain client trusts the Enterprise Root CA |
| `02-web01-https-trusted-page.png` | HTTPS page trusted from Windows client |
| `03-web01-certificate-issued-by-homelab-ca.png` | Certificate issued by HOMELAB-SRV-CA01-CA |
| `04-zabbix-srv-ca01-certsvc-trigger-ok.png` | Zabbix trigger for CertSvc service |

## Skills Demonstrated

- Windows Server administration
- Active Directory integration
- Active Directory Certificate Services
- Enterprise Root CA deployment
- Certificate template configuration
- Certificate Signing Request generation
- Linux/Nginx HTTPS configuration
- Internal DNS and certificate name validation
- Windows certificate trust validation
- Zabbix monitoring of a critical Windows service
- Security documentation and production considerations

## Final Result

This lab validates an internal Microsoft PKI scenario from end to end.

A Windows Server AD CS Enterprise Root CA was deployed, a Web Server certificate template was created, a certificate was issued for a Linux/Nginx server, HTTPS was configured and trusted access was validated from a Windows domain client.

The Certificate Authority was also monitored in Zabbix with a trigger on the CertSvc service.
