# Certificate Template

## Objective

The objective of this step was to create a dedicated Web Server certificate template for internal HTTPS certificates.

## Template Source

The built-in Web Server template was duplicated and customized.

## Template Settings

| Setting | Value |
|---|---|
| Template display name | Homelab Web Server |
| Template name | HomelabWebServer |
| Validity period | 2 years |
| Renewal period | 6 weeks |
| Subject name | Supplied in the request |
| Intended purpose | Server Authentication |

## Why Subject Name Is Supplied in the Request

The certificate request was generated on the Linux web server.

Because the CSR contains the requested subject and Subject Alternative Names, the template was configured to allow the subject name to be supplied in the request.

This allowed the Linux server to request a certificate for:

| Name type | Value |
|---|---|
| DNS | web01.homelab.local |
| DNS | web01 |
| IP | 10.10.30.10 |

## Publishing the Template

The template was published on the Certification Authority so it could be used when submitting certificate requests.

The published template was then used with certreq to issue the certificate for the Linux/Nginx server.

## Security Note

Allowing the subject name to be supplied in the request is useful for this lab, but in production it should be controlled carefully.

Only trusted administrators or specific groups should have enrollment permissions on this template.
