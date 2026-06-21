# AD CS Installation

## Objective

The objective of this step was to deploy a dedicated Windows Server Certificate Authority integrated with Active Directory.

## Server

| Setting | Value |
|---|---|
| Hostname | SRV-CA01 |
| IP address | 10.10.20.90 |
| Domain | homelab.local |
| Role | Active Directory Certificate Services |
| CA type | Enterprise Root CA |

## Windows Server Preparation

SRV-CA01 was installed as a dedicated Windows Server VM, configured with a static IP address and joined to the homelab.local Active Directory domain.

The server uses SRV-AD01 as its DNS server.

## AD CS Role Installation

The AD CS Certification Authority role was installed with management tools.

Command used:

    Install-WindowsFeature ADCS-Cert-Authority -IncludeManagementTools

## Enterprise Root CA Configuration

The server was configured as an Enterprise Root CA.

Command used:

    Install-AdcsCertificationAuthority `
      -CAType EnterpriseRootCA `
      -CACommonName "HOMELAB-SRV-CA01-CA" `
      -KeyLength 2048 `
      -HashAlgorithmName SHA256 `
      -CryptoProviderName "RSA#Microsoft Software Key Storage Provider" `
      -ValidityPeriod Years `
      -ValidityPeriodUnits 10

## CA Settings

| Setting | Value |
|---|---|
| CA name | HOMELAB-SRV-CA01-CA |
| CA type | Enterprise Root CA |
| Key length | 2048-bit RSA |
| Hash algorithm | SHA256 |
| Validity period | 10 years |

## Validation

The Certificate Services service was verified as running.

Command used:

    Get-Service CertSvc

The CA was also tested with certutil.

Command used:

    certutil -config "SRV-CA01\HOMELAB-SRV-CA01-CA" -ping

## Domain Client Trust Validation

On a domain-joined Windows client, the Enterprise Root CA was visible in the enterprise root store.

Command used:

    certutil -store -enterprise Root | findstr /i "HOMELAB"

This confirms that the CA certificate was published through Active Directory and trusted by domain clients.
