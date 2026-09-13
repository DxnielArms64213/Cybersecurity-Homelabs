# Overall Architecture

## Overview

The Enterprise SOC Homelab is designed to simulate a small enterprise environment containing a domain controller, endpoint, security workstation and central security monitoring server.

The environment is intended to provide hands-on experience with security monitoring, detection, investigation and incident response.

## Current and Planned Architecture

```text
                         ENTERPRISE SOC HOMELAB

                              Kali Linux
                         Security/Attack Workstation
                            192.168.50.20
                                  │
                                  │
                           VMware VMnet1
                          192.168.50.0/24
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
       Windows Server       Windows Client       Ubuntu Server
        192.168.50.30       192.168.50.40        192.168.50.10
        AD + DNS              Endpoint           Future Wazuh
       Domain Controller                            Server
```

## Main Components

### Kali Linux

Kali Linux acts as the security and attack workstation.

It will be used to:

* Perform reconnaissance
* Test the security of the lab
* Simulate controlled attacks
* Generate suspicious activity
* Test whether security monitoring detects the activity

All testing will be performed against systems within the authorised lab environment.

### Windows Server

Windows Server acts as the central Windows infrastructure server.

It provides:

* Active Directory Domain Services
* Domain Controller functionality
* DNS
* Domain user management
* Security groups
* Group Policy

The server currently uses:

`192.168.50.30`

### Windows Client

The Windows Client will act as a normal enterprise endpoint.

It will eventually:

* Join the Active Directory domain
* Receive Group Policy
* Generate Windows security events
* Run Sysmon
* Run the Wazuh Agent
* Provide endpoint telemetry for the SOC

Planned IP address:

`192.168.50.40`

### Ubuntu Server

Ubuntu Server is dedicated to becoming the central Wazuh server.

It will eventually receive security telemetry from the Windows Client and provide:

* Security monitoring
* Detection
* Alerting
* Log analysis
* Endpoint visibility

Current IP address:

`192.168.50.10`

Wazuh has not yet been installed.

## SOC Data Flow

The intended security monitoring pipeline is:

```text
                 Security Testing
                       │
                       ▼
                  Windows Client
                       │
              ┌────────┴────────┐
              │                 │
      Windows Event Logs      Sysmon
              │                 │
              └────────┬────────┘
                       ▼
                 Wazuh Agent
                       │
                       ▼
                Wazuh Server
                       │
                       ▼
                Detection Rules
                       │
                       ▼
                     Alert
                       │
                       ▼
                   Triage
                       │
                       ▼
                Investigation
                       │
                       ▼
              Incident Response
```

## Purpose of the Architecture

The purpose of this architecture is to recreate the basic workflow of a security operations environment.

Rather than simply installing security tools, the project will demonstrate the complete process:

```text
Attack
  ↓
Telemetry
  ↓
Detection
  ↓
Alert
  ↓
Triage
  ↓
Investigation
  ↓
MITRE ATT&CK Mapping
  ↓
Containment
  ↓
Remediation
  ↓
Incident Report
```

This allows the project to demonstrate practical cybersecurity skills rather than simply showing that several virtual machines and security tools were installed.
