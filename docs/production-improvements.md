# Production Improvements

## Objective

This document lists improvements that would be recommended before using this design in a production environment.

## Two-Tier PKI

This lab uses a single Enterprise Root CA for simplicity.

A production design should normally use:

| CA Type | Purpose |
|---|---|
| Offline Root CA | Long-term root of trust kept offline |
| Online Issuing CA | Issues certificates to users, computers and services |

This reduces the exposure of the root private key.

## Stronger Key and Crypto Settings

Depending on organizational requirements, production deployments may use stronger key lengths and stricter cryptographic standards.

Examples:

- 3072-bit or 4096-bit RSA keys.
- Hardware Security Module for CA key protection.
- Strict certificate validity periods.
- Regular cryptographic review.

## CRL and AIA Planning

Certificate revocation and chain building should be planned before production deployment.

Important components:

| Component | Purpose |
|---|---|
| CRL | Certificate Revocation List |
| AIA | Authority Information Access |
| OCSP | Online Certificate Status Protocol |

Clients must be able to access revocation and CA chain information.

## Enrollment Control

Certificate templates should use least privilege.

Only required users, computers or groups should have enrollment permissions.

Templates that allow the subject name to be supplied in the request should be especially restricted.

## Monitoring and Backup

Production PKI should include:

- CA service monitoring.
- Disk and system monitoring.
- Certificate expiration monitoring.
- CA backup.
- Private key backup protection.
- Documented recovery procedure.
- Audit log monitoring.

## Automation

Future improvements could include:

- GPO-based certificate auto-enrollment.
- Computer certificate deployment.
- Web server certificate renewal process.
- Integration with 802.1X / EAP-TLS lab scenarios.
- Certificate expiration alerts in Zabbix.
