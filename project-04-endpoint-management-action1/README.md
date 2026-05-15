# Project 04 - Endpoint Management with Action1

## Overview

This project provides a brief and practical introduction to endpoint management concepts using **Action1** in a controlled Blue Team home lab environment.

The goal was not to build a complex enterprise endpoint management architecture. Instead, this project was designed as a portfolio-focused exercise to demonstrate hands-on technical skills with a real endpoint management platform. Action1 was used to validate and document core capabilities such as endpoint onboarding, asset inventory, software inventory, patch visibility, vulnerability visibility, remote actions, reporting, and basic Linux endpoint validation.

This project is part of a larger Blue Team Home Lab portfolio and complements previous projects focused on firewall/IDS monitoring, vulnerability scanning, and SIEM operations.

---

## Lab Context

The lab environment is isolated and used only for learning, documentation, and portfolio development.

| Asset | Role | IP Address |
|---|---|---|
| pfSense | LAN Gateway / Firewall | 172.30.2.1 |
| Kali Linux | Linux endpoint / validation system | 172.30.2.100 |
| Metasploitable2 | Vulnerable Linux target | 172.30.2.101 |
| Metasploitable3 | Vulnerable Windows target | 172.30.2.102 |
| Wazuh Server | SIEM platform from previous project | 172.30.2.104 |
| Windows 11 Endpoint | Primary managed endpoint | 172.30.2.105 or current assigned IP |
| Action1 Console | Endpoint management platform | Cloud-based console |

> Note: Wazuh and Nessus were not the focus of this project. They were used in previous projects and are intentionally kept out of scope here.

---

## Project Objective

The objective of this project was to demonstrate a basic endpoint management workflow using Action1.

The project focused on the following areas:

- Action1 console validation
- Windows 11 endpoint onboarding
- Local agent validation
- Endpoint asset inventory
- Installed software visibility
- Patch and missing update visibility
- Endpoint-based vulnerability visibility
- Safe remote script execution
- Built-in reporting and compliance views
- Kali Linux agent validation

This project should be understood as a practical overview of endpoint management concepts, not as a full-scale enterprise deployment.

---

## Scope

### In Scope

- Validate access to the Action1 console
- Validate the Windows 11 endpoint as the primary managed endpoint
- Confirm the Action1 Agent service is running locally on Windows
- Review endpoint details collected by Action1
- Review installed software inventory
- Review update approval and missing update information
- Review vulnerability findings related to installed software and patch status
- Execute a safe read-only remote script
- Review built-in reports
- Validate Kali Linux as an additional Linux endpoint

### Out of Scope

- Production patch deployment
- Enterprise policy enforcement
- SIEM log correlation
- Wazuh alerting
- Nessus vulnerability scanning
- Exploitation or offensive testing
- Active Directory or domain-based endpoint management
- Complex compliance framework mapping
- Metasploitable3 endpoint onboarding

---

## Why Action1?

Action1 was selected because it provides practical endpoint management features that are useful for demonstrating Blue Team and Security Operations concepts, including:

- Endpoint visibility
- Patch management
- Software inventory
- Vulnerability remediation visibility
- Remote actions
- Reporting
- Agent-based endpoint control

For portfolio purposes, Action1 allowed the project to show how endpoint management tools can help security teams understand device posture, identify missing updates, review installed software, and execute controlled administrative actions.

---

## Project Structure

```text
project-04-endpoint-management-action1/
│   README.md
│
└───evidence
    ├───01-console-overview
    │       01-action1-dashboard-overview.png
    │       02-action1-platform-navigation-menu.png
    │
    ├───02-endpoint-onboarding
    │       01-windows-endpoint-visible-in-action1.png
    │       02-windows-agent-service-running.png
    │       03-windows-os-details-powershell.png
    │       04-windows-system-details-powershell.png
    │
    ├───03-asset-inventory
    │       01-windows-endpoint-general-details.png
    │
    ├───04-software-inventory
    │       01-installed-software-overview.png
    │
    ├───05-patch-management
    │       01-update-approval-overview.png
    │       02-update-details-example.png
    │
    ├───06-vulnerability-management
    │       01-vulnerabilities-overview.png
    │       02-vulnerability-details-example.png
    │
    ├───07-automation-and-remote-actions
    │       01-run-script-initial-configuration.png
    │       02-run-script-read-only-inventory-script.png
    │       03-run-script-execution-result.png
    │       04-remote-action-history.png
    │
    ├───08-reports-and-compliance
    │       01-built-in-reports-menu.png
    │       02-managed-endpoints-report.png
    │       03-software-inventory-report.png
    │       04-patch-management-report.png
    │       05-vulnerability-management-report.png
    │
    └───09-linux-validation
            01-action1-agent-deployment-options.png
            02-linux-agent-installation-instructions.png
            03-kali-system-validation.png
            04-kali-agent-installation-attempt.png
            05-kali-visible-in-action1.png
```

---

## Phase 1 - Console Overview

The first phase validated access to the Action1 console and documented the main platform areas available for endpoint management.

Evidence collected:

```text
evidence/01-console-overview/01-action1-dashboard-overview.png
evidence/01-console-overview/02-action1-platform-navigation-menu.png
```

This phase demonstrated that the console provides visibility into endpoint status, vulnerabilities, missing updates, compliance views, reports, automations, and agent deployment options.

---

## Phase 2 - Windows Endpoint Onboarding

The Windows 11 system was used as the primary managed endpoint for this project.

Evidence collected:

```text
evidence/02-endpoint-onboarding/01-windows-endpoint-visible-in-action1.png
evidence/02-endpoint-onboarding/02-windows-agent-service-running.png
evidence/02-endpoint-onboarding/03-windows-os-details-powershell.png
evidence/02-endpoint-onboarding/04-windows-system-details-powershell.png
```

The Action1 console confirmed that the Windows endpoint was visible and connected. Local PowerShell validation was also performed to confirm that the Action1 Agent service was running on the endpoint.

PowerShell commands used:

```powershell
Get-Service | Where-Object {$_.Name -like "*Action1*" -or $_.DisplayName -like "*Action1*"}

Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, BuildNumber, OSArchitecture

Get-CimInstance Win32_ComputerSystem | Select-Object Manufacturer, Model, TotalPhysicalMemory, Domain, Workgroup
```

This phase validated both cloud console visibility and local endpoint agent operation.

---

## Phase 3 - Asset Inventory

Action1 was used to review asset information collected from the managed Windows endpoint.

Evidence collected:

```text
evidence/03-asset-inventory/01-windows-endpoint-general-details.png
```

The asset inventory view provided endpoint details such as:

- Hostname
- User
- Connection status
- Operating system
- Platform
- Architecture
- Agent version
- Last seen time
- Last boot time
- Agent installation date
- Manufacturer
- CPU
- GPU

This demonstrates how endpoint management tools can help teams maintain visibility into managed assets.

---

## Phase 4 - Software Inventory

The installed software inventory was reviewed through the Action1 console.

Evidence collected:

```text
evidence/04-software-inventory/01-installed-software-overview.png
```

The software inventory view provided information such as:

- Software name
- Vendor
- Installed version
- Newest available update
- Install type
- Update status
- Platform
- Endpoint count

This phase demonstrated how endpoint management platforms can support software visibility and help identify applications that may require updates or review.

---

## Phase 5 - Patch Management

The update approval and patch visibility features were reviewed without applying production-style patch enforcement.

Evidence collected:

```text
evidence/05-patch-management/01-update-approval-overview.png
evidence/05-patch-management/02-update-details-example.png
```

The patch management view helped identify available updates and related details such as:

- Update name
- Version
- Release date
- Approval status
- Security severity
- Related vulnerabilities
- Update type
- Update source
- SLA compliance

This phase focused on visibility and decision support, not on deploying updates across a production environment.

---

## Phase 6 - Vulnerability Management Visibility

Action1 was used to review endpoint-based vulnerability findings related to installed software and missing patches.

Evidence collected:

```text
evidence/06-vulnerability-management/01-vulnerabilities-overview.png
evidence/06-vulnerability-management/02-vulnerability-details-example.png
```

The vulnerability view provided information such as:

- CVE identifier
- CVSS score
- CISA KEV status
- Published date
- Remediation status
- Vulnerable software
- Affected endpoints

This phase is different from the external vulnerability scanning performed in Project 02 with Nessus. In this project, the focus was endpoint-based vulnerability visibility and remediation context through an endpoint management platform.

---

## Phase 7 - Automation and Remote Actions

A safe read-only remote script was executed through Action1 to validate remote action capability.

Evidence collected:

```text
evidence/07-automation-and-remote-actions/01-run-script-initial-configuration.png
evidence/07-automation-and-remote-actions/02-run-script-read-only-inventory-script.png
evidence/07-automation-and-remote-actions/03-run-script-execution-result.png
evidence/07-automation-and-remote-actions/04-remote-action-history.png
```

The script was designed only to collect basic endpoint information and confirm remote execution. It did not modify files, services, users, registry settings, or security controls.

Script used:

```powershell
$hostname = hostname
$currentUser = whoami
$os = Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, BuildNumber, OSArchitecture
$computer = Get-CimInstance Win32_ComputerSystem | Select-Object Manufacturer, Model, Domain, Workgroup

Write-Output "=== Action1 Remote Script Validation ==="
Write-Output "Hostname: $hostname"
Write-Output "Current user context: $currentUser"
Write-Output ""
Write-Output "=== Operating System ==="
$os
Write-Output ""
Write-Output "=== Computer System ==="
$computer
```

This phase demonstrated controlled remote administration using a safe validation script.

---

## Phase 8 - Reports and Compliance

Built-in reporting capabilities were reviewed to understand how Action1 can support endpoint management operations.

Evidence collected:

```text
evidence/08-reports-and-compliance/01-built-in-reports-menu.png
evidence/08-reports-and-compliance/02-managed-endpoints-report.png
evidence/08-reports-and-compliance/03-software-inventory-report.png
evidence/08-reports-and-compliance/04-patch-management-report.png
evidence/08-reports-and-compliance/05-vulnerability-management-report.png
```

Reports reviewed included:

- Managed endpoints
- Software inventory
- Patch management
- Vulnerability management
- Built-in reporting categories

This phase showed how endpoint management tools can support operational reporting and provide structured visibility into managed assets.

---

## Phase 9 - Linux Endpoint Validation

Kali Linux was used as an additional validation endpoint to test Linux agent deployment and visibility in Action1.

Evidence collected:

```text
evidence/09-linux-validation/01-action1-agent-deployment-options.png
evidence/09-linux-validation/02-linux-agent-installation-instructions.png
evidence/09-linux-validation/03-kali-system-validation.png
evidence/09-linux-validation/04-kali-agent-installation-attempt.png
evidence/09-linux-validation/05-kali-visible-in-action1.png
```

The Kali system was validated locally before attempting deployment using commands such as:

```bash
hostname
ip addr show
cat /etc/os-release
uname -a
```

After the installation attempt, Kali Linux appeared in Action1, confirming that Linux endpoint visibility was successfully validated in the lab.

This was included as an additional compatibility validation phase and not as the main focus of the project.

---

## Technical Decisions

### Windows 11 as the Primary Endpoint

Windows 11 was selected as the primary endpoint because it represents a common enterprise workstation use case and was already available in the lab.

### Kali Linux as Additional Validation

Kali Linux was included to validate Linux agent deployment and endpoint visibility. It was not treated as the main managed endpoint because the project focus was a basic endpoint management workflow rather than full multi-platform administration.

### Metasploitable3 Out of Scope

Metasploitable3 was intentionally kept out of scope for this project. It was already used as a vulnerable target in previous lab projects and does not represent the cleanest choice for demonstrating endpoint management workflows.

### No Production Patch Deployment

Patch management was reviewed from a visibility and approval perspective only. The project did not attempt to simulate a complete production patch deployment lifecycle.

### No SIEM Integration

Wazuh and SIEM monitoring were intentionally excluded from this project. The focus here was endpoint management, inventory, patch visibility, vulnerability visibility, remote actions, and reporting.

---

## Skills Demonstrated

This project demonstrates practical familiarity with:

- Endpoint management concepts
- Agent-based endpoint onboarding
- Endpoint inventory validation
- Software inventory review
- Patch visibility and update approval concepts
- Endpoint-based vulnerability visibility
- Safe remote script execution
- Built-in reporting review
- Documentation of technical scope and limitations
- Lab-based security operations workflow

---

## Project Summary

This project provided a brief but practical overview of endpoint management using Action1.

The main goal was to demonstrate technical competence with an endpoint management tool in a Blue Team home lab environment. The project showed how a security or IT operations team can use an endpoint platform to onboard devices, review asset information, inspect installed software, identify missing updates, review vulnerability context, execute safe remote actions, and use reporting for operational visibility.

This was intentionally kept simple and portfolio-focused. It is not a full enterprise endpoint management deployment, but it demonstrates important practical concepts that are relevant to Blue Team, SOC, IT operations, and security operations roles.

---

## Future Improvements

Possible improvements for future iterations:

- Add more Windows endpoints
- Add a domain-joined endpoint
- Test patch approval and deployment in a controlled snapshot-based environment
- Create endpoint groups by operating system or role
- Compare patch visibility between Action1 and external scanners
- Build a simple compliance checklist
- Export reports for documentation
- Add GIFs or short recordings for portfolio presentation
- Test additional Linux distributions

---

## Disclaimer

This project was performed in an isolated home lab environment for educational and portfolio purposes only. No production systems were used.
