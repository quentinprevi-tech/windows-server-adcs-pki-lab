# Architecture

## Overview

This lab is hosted on a Proxmox VE homelab and uses an Active Directory domain with segmented networks managed by OPNsense.

The PKI lab focuses on deploying a Microsoft AD CS Enterprise Root CA and validating certificate-based HTTPS for an internal Linux/Nginx server.

## Main Components

| Component | Hostname | IP Address | Role |
|---|---|---:|---|
| Domain Controller | SRV-AD01 | 10.10.20.10 | Active Directory and DNS |
| Certificate Authority | SRV-CA01 | 10.10.20.90 | AD CS Enterprise Root CA |
| Web server | web01 | 10.10.30.10 | Debian/Nginx HTTPS server |
| Client | win11-client-lab | 10.10.10.105 | Domain-joined Windows validation client |
| Monitoring | Zabbix | 10.10.20.80 | Infrastructure monitoring |

## Network Segmentation

| Network | Subnet | Purpose |
|---|---|---|
| LAN | 10.10.10.0/24 | Client machines |
| SERVERS | 10.10.20.0/24 | Domain services, CA and monitoring |
| DMZ | 10.10.30.0/24 | Published/internal web server |

## DNS

Internal DNS is provided by SRV-AD01.

The web server is published in the internal DNS zone:

| DNS Record | IP Address |
|---|---:|
| web01.homelab.local | 10.10.30.10 |

This DNS name is also included in the certificate Subject Alternative Name field.

## Certificate Trust Flow

1. SRV-CA01 is configured as an Enterprise Root CA.
2. The CA certificate is published in Active Directory.
3. Domain-joined clients automatically trust the Enterprise Root CA.
4. web01 receives a certificate issued by SRV-CA01.
5. Windows clients trust `https://web01.homelab.local` because the issuing CA is trusted.

## Simplified Architecture Diagram

Windows 11 Client  
10.10.10.105  
↓ HTTPS trusted validation  
web01.homelab.local / 10.10.30.10  
↓ Certificate issued by  
SRV-CA01 / 10.10.20.90  
↓ Integrated with  
SRV-AD01 / 10.10.20.10
