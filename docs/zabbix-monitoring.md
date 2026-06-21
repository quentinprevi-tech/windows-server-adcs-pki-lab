# Zabbix Monitoring

## Objective

The objective of this step was to monitor the Certificate Authority server and detect if the AD CS service stops.

## Monitored Server

| Setting | Value |
|---|---|
| Hostname | SRV-CA01 |
| IP address | 10.10.20.90 |
| Monitoring platform | Zabbix |
| Agent | Zabbix Agent for Windows |

## Zabbix Host Configuration

SRV-CA01 was added as a monitored Windows host in Zabbix.

| Setting | Value |
|---|---|
| Host name | SRV-CA01 |
| Interface | 10.10.20.90:10050 |
| Template | Windows by Zabbix agent |
| Availability | ZBX green |

## Network Access

A temporary OPNsense rule was added to allow SRV-CA01 web access for the Zabbix Agent download.

The rule was removed after installation to keep the Certificate Authority server restricted.

## Monitored Service

The AD CS service was monitored using its Windows service name.

| Service | Value |
|---|---|
| Service name | CertSvc |
| Display purpose | Active Directory Certificate Services |
| Severity | High |

## Trigger

A dedicated Zabbix trigger was created to alert if the Certificate Services service is not running.

Trigger name:

    AD CS Certificate Services is not running

Trigger logic:

    State of service "CertSvc" is not running

## Validation

The host availability was confirmed with a green ZBX status.

The CertSvc trigger was configured and shown as OK while the AD CS service was running.

## Why This Matters

A Certificate Authority is a critical infrastructure component.

If the CA service stops, certificate enrollment, renewal and certificate management operations may be impacted.

Monitoring the CertSvc service provides early detection of CA availability issues.
