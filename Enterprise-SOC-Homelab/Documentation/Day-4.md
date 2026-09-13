# Enterprise SOC Homelab — Day 04

## Overview

Today I continued configuring the Windows Server portion of the Enterprise SOC Homelab.

The main focus was completing the initial Active Directory structure and beginning to configure security auditing through Group Policy.

## Work Completed

### Windows Server

Confirmed the existing Windows Server configuration:

* Server hostname configured
* Static IP configured as `192.168.50.30`
* Active Directory Domain Services installed
* Server promoted to a Domain Controller
* Active Directory domain created
* DNS configured

### Active Directory Structure

Created the following custom Organizational Units:

```text
Lab-Users
Lab-Computers
Lab-Servers
Lab-Groups
```

These provide a simple organisational structure for the lab and will allow Group Policy to be applied to specific groups of machines.

### User and Group Management

Created a domain user:

```text
labuser
```

The user was placed inside:

```text
Lab-Users
```

Created the security group:

```text
SOC-Analysts
```

The group was configured as a Global Security group.

`labuser` was then added as a member of `SOC-Analysts`.

This provides practical experience with Active Directory account and group management and will allow permissions to be assigned through groups rather than directly to individual accounts.

## Group Policy

Created a Group Policy Object:

```text
Lab - Security Baseline
```

The GPO was linked to:

```text
Lab-Computers
```

The following auditing policies were configured:

* Audit Process Creation
* Audit Logon
* Audit Logoff
* Audit User Account Management
* Audit Security Group Management
* Audit Audit Policy Change

Successful and failed events were enabled where appropriate.

### Purpose of the Auditing

The auditing configuration is intended to generate useful Windows security telemetry for the SOC.

Examples include:

* Process execution
* Successful authentication
* Failed authentication
* User account changes
* Security group changes
* Changes to audit policies

These events will eventually be collected and analysed using Wazuh.

## Current Architecture

```text
Windows Server
192.168.50.30
│
├── Active Directory
├── DNS
├── Users
│   └── labuser
├── Security Groups
│   └── SOC-Analysts
└── Group Policy
    └── Lab - Security Baseline
```

## Next Steps

The next stage will be creating the Windows Client VM.

The planned client configuration is:

```text
Hostname: WIN-CLIENT
IP: 192.168.50.40
DNS: 192.168.50.30
Network: VMware VMnet1 Host-Only
```

The client will eventually be joined to the Active Directory domain and placed into `Lab-Computers`.

After that, Sysmon and the Wazuh Agent will be introduced to begin building the endpoint telemetry pipeline.
