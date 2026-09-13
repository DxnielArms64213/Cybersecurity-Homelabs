# Network Architecture

## Overview

The Enterprise SOC Homelab uses an isolated VMware Host-Only network to allow the virtual machines to communicate with each other without directly exposing the lab environment to the normal home network.

This provides a controlled environment for cybersecurity testing, monitoring and incident-response exercises.

## Network Configuration

| Setting      | Configuration     |
| ------------ | ----------------- |
| Network      | VMware VMnet1     |
| Network Type | Host-Only         |
| Subnet       | `192.168.50.0/24` |
| Subnet Mask  | `255.255.255.0`   |
| DHCP         | Disabled          |

DHCP is disabled so that important systems can be assigned predictable static IP addresses.

## IP Address Plan

| Device         | Role                        | IP Address      |
| -------------- | --------------------------- | --------------- |
| Windows Host   | VMware Host                 | `192.168.50.1`  |
| Ubuntu Server  | Future Wazuh Server         | `192.168.50.10` |
| Kali Linux     | Security/Attack Workstation | `192.168.50.20` |
| Windows Server | Domain Controller / DNS     | `192.168.50.30` |
| Windows Client | Domain Endpoint             | `192.168.50.40` |

## Network Architecture

```text
                         Windows Host
                         192.168.50.1
                              │
                              │
                       VMware VMnet1
                        Host-Only Network
                       192.168.50.0/24
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
   Kali Linux           Windows Server        Ubuntu Server
  192.168.50.20         192.168.50.30         192.168.50.10
 Security/Attack          AD + DNS             Future Wazuh
   Workstation            Controller              Server
                              │
                              │
                              ▼
                       Windows Client
                       192.168.50.40
                         Domain Endpoint
```

## Why This Architecture Was Chosen

### Host-Only Networking

Host-Only networking was selected because the SOC lab needs to remain isolated from the normal home network.

The virtual machines can communicate with each other and with the Windows host, while the lab is separated from the physical network.

This makes the environment suitable for controlled security testing.

### Static IP Addresses

Static IP addresses are used for important infrastructure systems.

This provides predictable addressing for services such as:

* Active Directory
* DNS
* Wazuh
* Windows endpoints
* Security testing

For example, the Windows Client will use the Windows Server at `192.168.50.30` for DNS and domain-related services.

### Network Isolation

Keeping the lab on its own subnet makes it possible to safely simulate suspicious activity and attacks against intentionally vulnerable or test systems without accidentally targeting devices on the normal home network.

## Future Network Flow

The eventual SOC data flow will look approximately like this:

```text
Windows Client
      │
      │ Windows Events
      │ + Sysmon
      ▼
Wazuh Agent
      │
      ▼
Ubuntu Wazuh Server
      │
      ▼
Detection / Alerts
      │
      ▼
SOC Investigation
```

