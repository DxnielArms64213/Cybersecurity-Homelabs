# Daily Log — 2026/09/10

## 🎯 Objectives

* Configure the Ubuntu Server VM
* Configure the isolated Host-Only network
* Give Ubuntu a persistent static IP
* Test network connectivity
* Prepare Ubuntu for the future Wazuh installation

## 🔨 Work Completed

* Configured the VMware Host-Only network using the `192.168.50.0/24` subnet
* Identified Ubuntu's network interface as `ens33`
* Configured Ubuntu with the static IP `192.168.50.20`
* Configured Netplan to use NetworkManager
* Successfully applied the network configuration
* Tested connectivity between Ubuntu and the Windows host
* Rebooted Ubuntu and confirmed the static IP persists

### Task 1 — Configure the Host-Only network

**What I did:**

* Configured VMware VMnet1 as a Host-Only network
* Set the subnet to `192.168.50.0/24`
* Disabled VMware DHCP
* Established an IP plan for the lab

**Why I did it:**

* The SOC lab needs an isolated network where the virtual machines can communicate with each other
* This provides a controlled environment for future security testing

### Task 2 — Configure Ubuntu's network interface

**What I did:**

* Identified the Ubuntu network interface as `ens33`
* Investigated the existing Netplan configuration
* Found that Ubuntu was using DHCP for IPv4
* Changed the configuration to use a static IPv4 address

**Why I did it:**

* The future Wazuh server needs a predictable IP address
* Other systems in the lab will need to know where to send their logs

### Task 3 — Configure Ubuntu's static IP

**What I did:**

* Assigned Ubuntu the static IP `192.168.50.20/24`
* Configured Netplan to use NetworkManager as its renderer
* Generated and applied the Netplan configuration successfully

**Why I did it:**

* A static IP prevents the Ubuntu server's address from changing
* This will make the server easier to configure when Wazuh is installed later

### Task 4 — Troubleshoot the network configuration

**What I did:**

* Investigated an error where Netplan attempted to use `systemd-networkd`
* Confirmed that Ubuntu was actually using NetworkManager
* Added `renderer: NetworkManager` to the Netplan configuration
* Successfully applied the corrected configuration

**Why I did it:**

* Netplan needed to use the correct networking backend
* Understanding and troubleshooting this helped ensure the server's network configuration was working correctly

### Task 5 — Test network connectivity

**What I did:**

* Tested connectivity between Ubuntu and the Windows host at `192.168.50.1`
* Confirmed that Windows could successfully ping Ubuntu at `192.168.50.20`
* Found that Ubuntu could not currently receive ping replies from the Windows host

**Why I did it:**

* Network connectivity needs to be verified before building the rest of the lab
* Testing both directions helps identify potential networking or firewall issues

### Task 6 — Verify the static IP after reboot

**What I did:**

* Shut down and restarted the Ubuntu VM
* Checked the `ens33` interface after the restart
* Confirmed that `192.168.50.20/24` was still assigned

**Why I did it:**

* This confirmed that the static IP configuration is persistent
* Ubuntu is now prepared for the next stage of the SOC lab

## 📝 Current Status

* ✅ Kali Linux installed
* ✅ Ubuntu Server installed
* ✅ VMware Host-Only network configured
* ✅ Ubuntu `ens33` identified
* ✅ Ubuntu static IP configured: `192.168.50.20/24`
* ✅ Static IP survives reboot
* ✅ Windows → Ubuntu connectivity confirmed
* ⚠️ Ubuntu → Windows ping still needs troubleshooting
* ⏳ Wazuh **not installed yet**
* ⏳ Windows Server not configured yet
* ⏳ Windows Client not configured yet
* ⏳ Active Directory not configured yet
* ⏳ Sysmon not configured yet
