# Sysmon Configuration

## Overview

Sysmon was configured on `WIN-CLIENT` using the **SwiftOnSecurity Sysmon configuration**.

The configuration determines which system activities Sysmon records and therefore controls the telemetry available for security monitoring and investigation.

A configuration file is important because enabling every available Sysmon event would generate a large amount of telemetry, much of which may not be useful for the SOC. The SwiftOnSecurity configuration provides a practical security-focused baseline.

## Configuration Source

The lab uses the **SwiftOnSecurity Sysmon configuration** as its initial configuration baseline.

The configuration focuses on security-relevant system activity, including process execution, network activity and other changes that can provide useful evidence during an investigation.

The configuration can be modified later as the SOC develops and additional detection requirements are identified.

## Key Telemetry

The configuration enables Sysmon to provide telemetry that can be useful for identifying suspicious activity.

Important telemetry categories include:

| Telemetry              | Security Value                                                                               |
| ---------------------- | -------------------------------------------------------------------------------------------- |
| Process Creation       | Identifies programs and commands executed on the endpoint                                    |
| Network Connections    | Provides visibility into processes communicating over the network                            |
| Process Termination    | Helps establish when processes stop running                                                  |
| File Activity          | Can provide evidence of files being created or modified                                      |
| Registry Activity      | Can help identify changes to Windows configuration and persistence mechanisms                |
| Driver / Image Loading | Provides visibility into executable and driver loading                                       |
| DNS Activity           | Can provide additional context around domain resolution where supported by the configuration |

The exact events generated depend on the Sysmon version and configuration currently deployed.

## Process Creation

Process creation is one of the most valuable forms of Sysmon telemetry for the SOC.

When a process is created, Sysmon can provide information about the executable and its execution context.

This can help analysts answer questions such as:

* What program was executed?
* When was it executed?
* Which process launched it?
* What command-line arguments were used?
* Which user account executed it?
* Where was the executable located?

This information can be particularly useful when investigating suspicious command execution or malware activity.

## Network Activity

Sysmon can provide network connection telemetry associated with processes.

This can help connect network activity to the process responsible for generating it.

For example:

```text
Process
   │
   └── Network Connection
           │
           ├── Source IP
           ├── Destination IP
           ├── Destination Port
           └── Process Information
```

This provides additional context beyond simply observing that a network connection occurred.

## Windows Event Log

Sysmon writes its events to the Windows Sysmon Operational event log.

The log can be accessed through:

```text
Applications and Services Logs
└── Microsoft
    └── Windows
        └── Sysmon
            └── Operational
```

The Wazuh Agent can collect relevant Sysmon events from this log and forward them to the Wazuh Manager for processing.

## Relationship With Windows Security Logs

Sysmon and Windows Security logging serve different purposes.

Windows Security logs provide information about security-related operating system activity, such as:

* User logons
* Authentication failures
* Account changes
* Group membership changes
* Security policy changes

Sysmon provides additional endpoint telemetry, such as:

* Process execution
* Network connections
* File activity
* Registry activity

Using both sources provides a more complete picture of activity occurring on the endpoint.

For example:

```text
Windows Security Log
        │
        ├── User authentication
        │
        ▼
     WIN-CLIENT
        ▲
        │
        ├── Process activity
        ├── Network activity
        └── System activity
        │
        ▼
      Sysmon
```

These sources can later be correlated by Wazuh during detection and investigation.

## Role in the SOC

Sysmon acts as an endpoint telemetry source rather than a standalone detection or response system.

The telemetry pipeline is:

```text
Endpoint Activity
       │
       ▼
     Sysmon
       │
       ▼
Sysmon Event Log
       │
       ▼
 Wazuh Agent
       │
       ▼
 Wazuh Manager
       │
       ▼
Detection Rules
       │
       ▼
Wazuh Alert
       │
       ▼
SOC Investigation
```

This allows Sysmon to provide the underlying evidence while Wazuh performs centralised collection, analysis and alerting.

## Configuration Management

The Sysmon configuration should be treated as part of the SOC detection infrastructure.

Changes should be documented because modifying the configuration can change the telemetry available to Wazuh and therefore affect detection capabilities.

Future configuration changes may be required when:

* New detection use cases are developed
* Additional attack techniques are tested
* Existing telemetry creates excessive noise
* New Sysmon features become relevant
* Detection rules require additional event data

Configuration changes should be tested before being incorporated into the main lab configuration.

## Current Status

The current Sysmon configuration is operational on `WIN-CLIENT`.

Current state:

* SwiftOnSecurity configuration deployed
* Sysmon generating telemetry
* Sysmon Operational log verified
* Process and system telemetry available
* Configuration ready for Wazuh integration

## Future Detection Use Cases

The telemetry generated by Sysmon will later be used to build detection scenarios within the SOC.

Potential use cases include:

* Suspicious PowerShell execution
* Command-line abuse
* Unusual process creation
* Suspicious parent-child process relationships
* Unexpected network connections
* Persistence-related activity
* Credential-access activity
* Malware execution
* Lateral movement activity

These detections will be developed alongside controlled attack simulations from Kali.

The goal is to move from simply collecting telemetry to using that telemetry to identify, investigate and respond to simulated security incidents.
