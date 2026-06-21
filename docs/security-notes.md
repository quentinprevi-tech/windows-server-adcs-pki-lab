# Security Notes

## Objective

This document summarizes security considerations for the AD CS / PKI lab.

## Private Key Protection

The private key generated on web01 must remain private.

The following files must never be committed to GitHub:

| File type | Reason |
|---|---|
| .key | Contains the private key |
| .pfx / .p12 | May contain certificate and private key |
| CA private key backup | Critical CA secret |
| Password files | Could expose protected keys or services |

## Certificate Files

Public certificates are less sensitive than private keys, but this repository only documents the process and validation steps.

The full certificate content is not required in the repository.

## Certificate Authority Exposure

SRV-CA01 should be treated as a sensitive server.

In this lab, Internet access was only temporarily allowed to download the Zabbix Agent, then removed.

This keeps the CA server more restricted.

## Template Security

The Homelab Web Server template allows the subject name to be supplied in the request.

This is useful for issuing certificates from Linux CSRs, but it should be controlled carefully.

Only trusted administrators or specific authorized groups should have enrollment rights.

## Production Considerations

In production, a more secure PKI design would usually include:

- Offline Root CA.
- Online Issuing CA.
- Stronger CA private key protection.
- Documented certificate revocation process.
- CRL and AIA availability planning.
- Regular CA backup.
- Auditing of certificate issuance.
- Restricted enrollment permissions.
- Monitoring and alerting for CA health.
