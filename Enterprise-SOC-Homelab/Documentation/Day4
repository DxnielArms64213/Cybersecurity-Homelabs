# Daily Log — 2026/09/13

## Objectives

* Configure the Windows Server Active Directory environment
* Create the initial Active Directory organisational structure
* Create a domain user account
* Create a security group for SOC analysts
* Configure a security baseline Group Policy
* Enable Windows security auditing
* Prepare the environment for the future Windows Client

## Work Completed

* Confirmed the existing Windows Server configuration
* Created the Active Directory organisational units:

  * `Lab-Users`
  * `Lab-Computers`
  * `Lab-Servers`
  * `Lab-Groups`
* Created the domain user `labuser`
* Placed `labuser` inside `Lab-Users`
* Created the `SOC-Analysts` security group
* Added `labuser` to `SOC-Analysts`
* Created the `Lab - Security Baseline` Group Policy Object
* Linked the GPO to `Lab-Computers`
* Configured several Windows security auditing policies
* Verified that the Group Policy was correctly linked
* Prepared the Active Directory environment for the future Windows Client

### Task 1 — Configure the Active Directory structure

**What I did:**

* Confirmed that Active Directory Domain Services was already installed
* Confirmed that the Windows Server was functioning as the Domain Controller
* Created the following custom organisational units:

  * `Lab-Users`
  * `Lab-Computers`
  * `Lab-Servers`
  * `Lab-Groups`

**Why I did it:**

* Organisational units provide a way to structure the lab environment
* They allow Group Policy to be applied to specific types of systems
* Separating users, computers, servers and groups will make the environment easier to manage as the lab grows

### Task 2 — Create the lab user

**What I did:**

* Created the domain user:

  * `labuser`
* Placed the account inside `Lab-Users`

**Why I did it:**

* A dedicated domain account provides a realistic user identity for the simulated enterprise environment
* The account can later be used to generate authentication and account-management events for security monitoring

### Task 3 — Create the SOC security group

**What I did:**

* Created the security group:

  * `SOC-Analysts`
* Configured it as a Global Security group
* Added `labuser` as a member

**Why I did it:**

* Security groups allow permissions to be managed through groups rather than individual users
* This provides a more realistic Active Directory structure
* The group can later be used when configuring permissions and access within the lab

### Task 4 — Configure the security baseline Group Policy

**What I did:**

* Created the Group Policy Object:

  * `Lab - Security Baseline`
* Linked the GPO to:

  * `Lab-Computers`

**Why I did it:**

* Group Policy provides centralised configuration for domain computers
* The future Windows Client can receive the security configuration automatically when it is placed inside `Lab-Computers`
* This provides a more realistic enterprise security-management environment

### Task 5 — Configure Windows security auditing

**What I did:**

Configured the following Advanced Audit Policy settings:

* Audit Process Creation
* Audit Logon
* Audit Logoff
* Audit User Account Management
* Audit Security Group Management
* Audit Audit Policy Change

Successful and failed events were enabled where appropriate.

**Why I did it:**

* These policies generate useful security telemetry
* Process creation can provide visibility into programs being executed
* Logon auditing can identify successful and failed authentication attempts
* User and group management auditing can identify account and privilege changes
* Audit Policy Change events can identify changes to security auditing configuration

These events will eventually provide useful data for the Wazuh monitoring system.

### Task 6 — Verify the Group Policy configuration

**What I did:**

* Confirmed that `Lab - Security Baseline` was linked to `Lab-Computers`
* Verified that the required auditing policies were configured
* Confirmed that the Windows Server configuration was ready for the next stage

**Why I did it:**

* Verifying the configuration helps prevent problems when the Windows Client is introduced
* The client will later inherit the security baseline when it is joined to the domain and placed inside `Lab-Computers`

## 📝 Current Status

* ✅ Kali Linux installed
* ✅ Ubuntu Server installed
* ✅ VMware Host-Only network configured
* ✅ Ubuntu static IP configured: `192.168.50.20/24`
* ✅ Windows Server installed
* ✅ Windows Server static IP configured: `192.168.50.30/24`
* ✅ Active Directory Domain Services configured
* ✅ Windows Server promoted to Domain Controller
* ✅ DNS configured
* ✅ Active Directory organisational structure created
* ✅ `labuser` domain account created
* ✅ `SOC-Analysts` security group created
* ✅ `labuser` added to `SOC-Analysts`
* ✅ `Lab - Security Baseline` GPO created
* ✅ Security auditing configured
* ✅ GPO linked to `Lab-Computers`
* ⏳ Windows Client not configured yet
* ⏳ Sysmon not configured yet
* ⏳ Wazuh **not installed yet**
* ⏳ Detection rules not configured yet
* ⏳ Incident-response testing not started yet

## Next Steps

The next stage will be creating the Windows Client VM.

The planned client configuration is:

```text
Hostname: WIN-CLIENT
IP: 192.168.50.40
DNS: 192.168.50.30
Network: VMware VMnet1 Host-Only
```

The Windows Client will then be joined to the Active Directory domain and placed inside `Lab-Computers`.

After the client is working correctly, Sysmon and the Wazuh Agent can be introduced to begin building the endpoint telemetry pipeline.
