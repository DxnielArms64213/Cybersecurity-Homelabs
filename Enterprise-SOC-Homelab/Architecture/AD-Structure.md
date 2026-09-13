# Active Directory Structure

## Overview

Active Directory provides centralised identity and management for the Windows systems within the Enterprise SOC Homelab.

The environment uses organisational units, domain users and security groups to replicate a basic enterprise directory structure.

## Active Directory Structure

```text
YOUR-DOMAIN
│
├── Lab-Users
│   └── labuser
│
├── Lab-Computers
│
├── Lab-Servers
│
└── Lab-Groups
    └── SOC-Analysts
        └── labuser
```

## Organisational Units

### Lab-Users

Contains user accounts belonging to the lab environment.

Current user:

```text
labuser
```

Separating users into their own OU makes the directory easier to manage and provides a dedicated location for future user accounts.

### Lab-Computers

This OU is intended to contain Windows endpoint computers.

The future Windows Client will be placed here.

This OU is also important because the security baseline Group Policy is linked to it.

### Lab-Servers

This OU is intended to contain server systems that are part of the lab environment.

It provides a separate location for server objects as the environment grows.

### Lab-Groups

Contains security groups used to manage permissions and access.

Current group:

```text
SOC-Analysts
```

## Domain User

A domain user named:

```text
labuser
```

was created and placed inside:

```text
Lab-Users
```

The account provides a realistic identity that can later be used to generate authentication and account-management activity.

## Security Group

The following Global Security group was created:

```text
SOC-Analysts
```

`labuser` was added as a member.

Using a security group allows permissions to be assigned to a role or group of users instead of individually configuring each account.

## Why This Structure Was Chosen

The structure provides a simple but realistic starting point for the lab.

It separates:

* Users
* Computers
* Servers
* Security groups

This will become increasingly useful as more systems and accounts are added.

It also provides a foundation for applying different Group Policy configurations to different parts of the environment.

## Future Expansion

As the SOC lab develops, additional users, computers and security groups can be added without redesigning the entire directory structure.

The Windows Client will eventually be placed inside:

```text
Lab-Computers
```

This will allow it to receive the security baseline Group Policy.
