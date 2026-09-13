# Network Architecture

## Network

Network: VMware VMnet1 Host-Only
Subnet: 192.168.50.0/24
DHCP: Disabled

## IP Address Plan

| Device | Role | IP Address |
|---|---|---|
| Windows Host | VMware Host | 192.168.50.1 |
| Kali Linux | Security/Attack Workstation | 192.168.50.20 |
| Ubuntu Server | Future Wazuh Server | 192.168.50.10 |
| Windows Server | Domain Controller / DNS | 192.168.50.30 |
| Windows Client | Domain Endpoint | 192.168.50.40 |
