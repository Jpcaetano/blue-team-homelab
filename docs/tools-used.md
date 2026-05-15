# Tools Used

> Consolidated tool inventory for the Blue Team home lab portfolio.

---

## Tool Stack Overview

```text
┌──────────────────────┐
│ Virtualization Layer │  VirtualBox
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ Network Layer        │  pfSense, Snort, tcpdump
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ Security Tools       │  Nessus, Wazuh, Action1, Nmap
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ Endpoint Layer       │  Windows 11, Kali Linux, Metasploitable targets
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ Documentation Layer  │  Git, Markdown, screenshots, reports
└──────────────────────┘
```

---

## Core Infrastructure

| Tool / Asset | Category | Purpose | Used In |
|---|---|---|---|
| VirtualBox | Virtualization | Hosts the lab virtual machines | All projects |
| pfSense | Firewall / Gateway | Internal lab gateway, firewall, and Snort host | Project 01, 02, 03 |
| Kali Linux | Testing / Validation | Scanner, traffic generator, monitored endpoint, Linux validation | Project 01, 02, 03, 04 |
| Metasploitable2 | Vulnerable Target | Intentionally vulnerable Linux target | Project 01, 02, 03 |
| Metasploitable3 | Vulnerable Target | Intentionally vulnerable Windows target | Project 02 |
| Windows 11 Host | Physical Host / Endpoint | Dashboard access, repository work, Action1 managed endpoint | Project 03, 04 |

---

## Security and Blue Team Tools

| Tool | Type | Main Use | Project Coverage |
|---|---|---|---|
| Snort | IDS | Detect controlled lab traffic and validate custom rules | Project 01, 03 |
| Nessus Essentials | Vulnerability Scanner | Identify vulnerabilities, severity, CVEs, and remediation guidance | Project 02 |
| Wazuh | SIEM / Log Monitoring | Collect logs, endpoint events, FIM alerts, and syslog events | Project 03 |
| Action1 | Endpoint Management | Endpoint inventory, patch visibility, vulnerability visibility, remote actions | Project 04 |
| Nmap | Network Scanner | Service discovery and controlled scan traffic | Project 01, 02, 03 |
| tcpdump | Packet Capture | Validate syslog forwarding and packet-level traffic | Project 03 |
| Netcat | Connectivity Testing | Validate service connection attempts | Project 01 |
| cURL | HTTP Testing | Generate controlled HTTP traffic | Project 01 |

---

## Endpoint and Operating System Tools

| Tool / Command | Platform | Purpose |
|---|---|---|
| PowerShell | Windows | Service validation, OS details, system inventory, safe scripting |
| Get-Service | Windows | Validate local agent services |
| Get-CimInstance | Windows | Collect OS and hardware details |
| Bash | Linux | General Linux command-line validation |
| ip addr | Linux | Validate network interfaces and IP addresses |
| uname | Linux | Validate kernel and system information |
| journalctl | Linux | Review Linux system logs and sudo activity |
| systemctl | Linux | Validate service status |

---

## Documentation and Portfolio Tools

| Tool | Purpose |
|---|---|
| Git | Version control |
| GitHub / GitLab | Portfolio hosting and repository presentation |
| Markdown | Technical documentation format |
| Screenshots | Evidence collection |
| PDF Reports | Exported vulnerability scan evidence |
| JSON Exports | Alert analysis and SIEM event review |
| CSV Exports | IDS alert evidence and structured results |

---

## Tool Coverage by Project

| Tool | P01 IDS | P02 Vuln Mgmt | P03 SIEM | P04 Endpoint Mgmt |
|---|:---:|:---:|:---:|:---:|
| VirtualBox | ✅ | ✅ | ✅ | ✅ |
| pfSense | ✅ | ✅ | ✅ | ◐ |
| Snort | ✅ | — | ✅ | — |
| Kali Linux | ✅ | ✅ | ✅ | ✅ |
| Metasploitable2 | ✅ | ✅ | ✅ | — |
| Metasploitable3 | ◐ | ✅ | ◐ | — |
| Nessus | — | ✅ | — | — |
| Wazuh | — | ◐ | ✅ | — |
| Action1 | — | — | — | ✅ |
| Nmap | ✅ | ✅ | ✅ | — |
| tcpdump | — | — | ✅ | — |
| PowerShell | — | — | ✅ | ✅ |
| Bash | ✅ | ✅ | ✅ | ✅ |

Legend:

| Symbol | Meaning |
|---|---|
| ✅ | Main tool or active part of the project |
| ◐ | Present in the environment, optional, or out of main scope |
| — | Not used in that project |

---

## Tool Categories Visual Map

```text
Network Visibility
├── pfSense
├── Snort
├── Nmap
└── tcpdump

Vulnerability Visibility
├── Nessus Essentials
├── Nmap
├── Metasploitable2
└── Metasploitable3

SIEM and Log Visibility
├── Wazuh
├── Windows Agent
├── Kali Agent
├── pfSense Syslog
└── Snort Alerts

Endpoint Management Visibility
├── Action1
├── Windows 11 Endpoint
├── Kali Linux validation
├── Patch visibility
└── Vulnerability visibility

Documentation
├── Markdown
├── Git
├── Screenshots
├── PDF reports
├── CSV exports
└── JSON exports
```

---

## Why These Tools Were Used

| Tool | Portfolio Value |
|---|---|
| pfSense | Shows basic firewall/gateway understanding |
| Snort | Shows IDS deployment and detection validation |
| Nessus | Shows vulnerability management workflow experience |
| Wazuh | Shows SIEM, log collection, and alert analysis practice |
| Action1 | Shows endpoint management and patch/vulnerability visibility concepts |
| Nmap | Shows service discovery and controlled validation skills |
| tcpdump | Shows ability to validate traffic and troubleshoot integrations |
| PowerShell | Shows Windows administration and endpoint validation skills |
| Bash | Shows Linux command-line confidence |
| Markdown/Git | Shows professional documentation and portfolio organization |

---

## Status

Active inventory.
