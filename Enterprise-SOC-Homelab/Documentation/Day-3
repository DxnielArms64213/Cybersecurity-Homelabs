# Daily Log — 2026/09/12

## Objectives

- Configure the Ubuntu server's static IP
- Fix connectivity issues between the lab machines
- Create the Windows Server 2022 VM
- Configure Windows Server networking
- Troubleshoot Windows Server connectivity
- Rename the server to `DC01`
- Install Active Directory Domain Services
- Promote the server to a Domain Controller
- Create the `soclab.local` Active Directory domain
- Configure DNS
- Verify that Active Directory and DNS are working

## Work Completed

- Configured Ubuntu with a persistent static IP
- Troubleshot connectivity between Ubuntu, Kali and the Windows host
- Identified and fixed Windows Defender Firewall ICMP blocking
- Created and installed Windows Server 2022
- Configured Windows Server with a static IP
- Troubleshot Windows Server connectivity
- Renamed Windows Server to `DC01`
- Installed Active Directory Domain Services
- Promoted `DC01` to a Domain Controller
- Created the `soclab.local` Active Directory forest
- Configured DNS through the Domain Controller
- Verified Active Directory Users and Computers
- Verified the `soclab.local` DNS zone

---

## Task 1 — Configure Ubuntu Static IP

### What I did

- Configured the Ubuntu server with the static IP address `192.168.50.20/24`
- Configured Netplan to use NetworkManager
- Changed DHCP for IPv4 from enabled to disabled
- Generated and applied the new Netplan configuration
- Rebooted the Ubuntu VM and verified that the static IP remained after reboot

### Why I did it

The Ubuntu machine will eventually become the dedicated Wazuh server.

A static IP is important for a server because other machines in the SOC lab need to know where to reliably find it.

The Ubuntu server will use:

- IP address: `192.168.50.20`
- Subnet mask: `255.255.255.0`

---

## Task 2 — Troubleshoot Ubuntu Network Connectivity

### What I did

Initially, the Ubuntu server could not successfully ping the Windows host at `192.168.50.1`.

However, the Windows host could successfully ping Ubuntu.

I checked the VMware Host-Only network configuration and confirmed that:

- VMnet1 was being used
- The subnet was `192.168.50.0/24`
- DHCP was disabled
- The Windows VMnet1 adapter was `192.168.50.1`
- Ubuntu was `192.168.50.20`
- The VMware virtual network itself was functioning

I then investigated the Windows Defender Firewall.

The relevant ICMPv4 Echo Request inbound rule was disabled.

I enabled the rule instead of disabling the Windows firewall completely.

### Result

Ubuntu was then able to successfully ping the Windows host:

- 4 packets sent
- 4 packets received
- 0% packet loss

Kali was also able to successfully ping the Windows host.

### Why I did it

This demonstrated that the VMware network itself was working correctly and that the problem was caused by the Windows firewall blocking ICMP traffic.

This was useful troubleshooting experience because a machine can be correctly configured on the network while its local firewall still prevents communication.

---

## Task 3 — Verify the Lab Network

### What I did

I tested communication between the lab machines.

The current network design is:

| Device | IP Address | Purpose |
|---|---|---|
| Windows Host | `192.168.50.1` | Host / VMware VMnet1 |
| Kali Linux | `192.168.50.10` | Security / Attack Workstation |
| Ubuntu Server | `192.168.50.20` | Future Wazuh Server |
| Windows Server | `192.168.50.30` | Domain Controller |
| Windows Client | `192.168.50.40` | Future Enterprise Endpoint |

The VMware network is:

`192.168.50.0/24`

DHCP is disabled so that the lab machines can use predictable static addresses.

### Connectivity verified

- Ubuntu -> Windows Host: successful
- Kali -> Windows Host: successful
- Kali -> Ubuntu: successful

### Why I did it

The SOC lab needs reliable communication between its components.

Before adding Active Directory, Wazuh and Windows endpoints, the underlying network needs to be working correctly.

---

## Task 4 — Create Windows Server 2022 VM

### What I did

Created a new Windows Server virtual machine in VMware.

The VM was configured with:

- Name: `Windows Server`
- Operating System: Windows Server 2022
- CPU: 2 cores
- RAM: 4–6 GB
- Storage: 60 GB
- Network: VMware Host-Only / VMnet1
- Installation media: Windows Server 2022 Evaluation ISO

The Windows Server 2022 Evaluation ISO was downloaded from Microsoft's official Evaluation Center.

During installation I selected:

**Windows Server 2022 Standard Evaluation (Desktop Experience)**

I used a custom installation and installed Windows onto the virtual disk.

### Why I did it

The Windows Server will become the central server for the simulated enterprise environment.

It will provide:

- Active Directory
- Domain authentication
- DNS
- Domain management
- User and computer management

This will allow the lab to simulate an actual small enterprise environment rather than just isolated machines.

---

## Task 5 — Configure Windows Server Networking

### What I did

After installing Windows Server, I checked the network configuration.

Because DHCP was disabled on the VMware Host-Only network, Windows Server did not initially have a valid lab IP address.

I manually configured the network adapter with:

- IP address: `192.168.50.30`
- Subnet mask: `255.255.255.0`
- Default gateway: blank
- Preferred DNS server: `192.168.50.30`

### Why I did it

The Windows Server needs a predictable IP address because it will become the Domain Controller and DNS server.

The server will therefore be permanently assigned:

`192.168.50.30`

---

## Task 6 — Troubleshoot Windows Server Connectivity

### What I did

After configuring the static IP, Windows Server could successfully communicate with the Windows host.

However, Ubuntu could not initially ping the Windows Server.

Ubuntu sent four ICMP packets to:

`192.168.50.30`

but received no replies:

- 4 packets sent
- 0 packets received
- 100% packet loss

I investigated the Windows Defender Firewall with Advanced Security.

The issue was similar to the earlier Ubuntu/Windows Host problem: Windows was blocking ICMP Echo Requests.

I enabled the relevant inbound ICMPv4 Echo Request firewall rule rather than disabling the Windows firewall.

### Result

Ubuntu was then able to successfully ping Windows Server:

- 4 packets sent
- 4 packets received
- 0% packet loss

### Why I did it

This confirmed that Windows Server was correctly connected to the VMware network.

It also reinforced an important networking and security concept:

> Network connectivity can be working correctly while a host firewall prevents specific types of traffic.

Instead of disabling the firewall completely, I allowed only the required ICMP traffic.

This is more appropriate for a security-focused lab.

---

## Task 7 — Rename Windows Server

### What I did

Renamed the Windows Server computer to:

`DC01`

The server was restarted to apply the new hostname.

### Why I did it

`DC01` stands for Domain Controller 01.

Using a meaningful hostname makes the enterprise environment easier to manage and understand.

It also makes the lab more realistic because enterprise environments commonly use structured naming conventions.

---

## Task 8 — Install Active Directory Domain Services

### What I did

Opened:

**Server Manager -> Manage -> Add Roles and Features**

Selected:

**Role-based or feature-based installation**

Selected the local server:

`DC01`

Installed:

**Active Directory Domain Services**

Accepted the required additional features and completed the installation.

### Why I did it

Active Directory Domain Services (AD DS) provides centralised management of users, computers, authentication and security policies within a Windows domain.

This will allow the lab to simulate an enterprise Windows environment.

---

## Task 9 — Promote DC01 to Domain Controller

### What I did

After installing AD DS, I used the Server Manager notification to:

**Promote this server to a domain controller**

Selected:

**Add a new forest**

Configured the root domain as:

`soclab.local`

Configured the NetBIOS domain name as:

`SOCLAB`

DNS Server was enabled.

Global Catalog was enabled.

The DSRM password was configured.

DNS delegation was left unchecked because this is a new isolated Active Directory forest.

The default database, log and SYSVOL paths were retained.

The prerequisite check completed successfully.

The server was then promoted to a Domain Controller and restarted.

### Why I did it

The Domain Controller is the central component of the simulated enterprise environment.

It provides:

- Active Directory
- Domain authentication
- User management
- Computer management
- DNS
- Centralised identity management

The new lab domain is:

`soclab.local`

---

## Task 10 — Verify Active Directory

### What I did

After the server restarted, I opened:

**Server Manager -> Tools -> Active Directory Users and Computers**

The domain was visible as:

`SOCLAB.LOCAL`

The default Active Directory containers were present, including:

- Builtin
- Computers
- Domain Controllers
- Users

### Why I did it

This confirmed that Active Directory was successfully installed and the Domain Controller was functioning.

---

## Task 11 — Verify DNS

### What I did

Opened:

**Server Manager -> Tools -> DNS**

Navigated to:

`DC01 -> Forward Lookup Zones`

The following DNS zone was present:

`soclab.local`

### Why I did it

DNS is essential for Active Directory.

Windows clients use DNS to locate services provided by the Domain Controller, including Active Directory services.

The Domain Controller will therefore provide DNS for the lab.

---

# Troubleshooting Summary

Several networking issues were encountered during this stage of the project.

## Issue 1 — Ubuntu could not ping Windows Host

### Cause

Windows Defender Firewall was blocking ICMP Echo Requests.

### Solution

Enabled the appropriate ICMPv4 Echo Request inbound rule.

### Result

Ubuntu -> Windows Host worked successfully.

---

## Issue 2 — Windows Server did not initially have a valid IP address

### Cause

DHCP was disabled on the VMware Host-Only network.

### Solution

Configured a static IP:

`192.168.50.30/24`

### Result

Windows Server received the correct lab IP.

---

## Issue 3 — Ubuntu could not ping Windows Server

### Cause

Windows Server's firewall was blocking ICMP Echo Requests.

### Solution

Enabled the appropriate ICMPv4 Echo Request inbound rule.

### Result

Ubuntu -> Windows Server worked successfully with 0% packet loss.

---

# Current Lab Architecture
                         WINDOWS HOST
                         192.168.50.1
                              |
                         VMware VMnet1
                         192.168.50.0/24
                              |
          +-------------------+-------------------+
          |                   |                   |
          |                   |                   |
       Kali Linux          Ubuntu Server       Windows Server
      192.168.50.10       192.168.50.20         192.168.50.30
      Attack Machine      Future Wazuh           DC01
                           Server                 Domain Controller
                                                   |
                                                   |
                                            soclab.local
                                                   |
                                                   |
                                            Active Directory
                                                   |
                                                   |
                                              DNS Server
