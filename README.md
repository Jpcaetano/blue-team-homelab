# Blue Team Home Lab Portfolio

![Status](https://img.shields.io/badge/status-active-success)
![Focus](https://img.shields.io/badge/focus-blue%20team-blue)
![Lab](https://img.shields.io/badge/environment-home%20lab-lightgrey)
![Documentation](https://img.shields.io/badge/documentation-evidence--based-informational)

> A practical Blue Team home lab portfolio focused on network security monitoring, vulnerability management, SIEM operations, endpoint monitoring, and endpoint management.

---

## Portfolio Snapshot

This repository documents a hands-on cybersecurity home lab built to demonstrate defensive security workflows using real tools, isolated lab networks, intentionally vulnerable targets, and structured evidence collection.

The goal of this portfolio is not to simulate a full enterprise environment, but to show practical technical ability across key Blue Team areas:

```text
Firewall / IDS  →  Vulnerability Management  →  SIEM Monitoring  →  Endpoint Management
```

Each project includes its own scope, methodology, screenshots, evidence files, and technical notes.

---

## High-Level Lab Architecture

```text
                         ┌──────────────────────────────┐
                         │ Windows 11 Host / Endpoint    │
                         │ - Git / Browser / Management  │
                         │ - Wazuh Agent                 │
                         │ - Action1 Agent               │
                         └──────────────┬───────────────┘
                                        │
                         Management / Host Access Network
                                192.168.6.0/24
                                        │
                ┌───────────────────────┴───────────────────────┐
                │                                               │
        ┌───────▼────────┐                              ┌───────▼────────┐
        │ Wazuh Dashboard│                              │ Action1 Console│
        │ Host Access    │                              │ Cloud Platform │
        └───────┬────────┘                              └────────────────┘
                │
                │
        ┌───────▼───────────────────────────────────────────────┐
        │                  Internal Lab Network                  │
        │                     172.30.2.0/24                      │
        └───────────────────────┬───────────────────────────────┘
                                │
                         ┌──────▼──────┐
                         │  pfSense    │
                         │ Firewall    │
                         │ Snort IDS   │
                         │ 172.30.2.1  │
                         └──────┬──────┘
                                │
       ┌────────────────────────┼────────────────────────┐
       │                        │                        │
┌──────▼──────┐        ┌────────▼────────┐       ┌───────▼────────┐
│ Kali Linux  │        │ Metasploitable2 │       │ Metasploitable3│
│ Scanner     │        │ Linux Target    │       │ Windows Target │
│ 172.30.2.100│        │ 172.30.2.101    │       │ 172.30.2.102   │
└──────┬──────┘        └─────────────────┘       └────────────────┘
       │
┌──────▼──────┐
│ Wazuh Server│
│ SIEM        │
│ 172.30.2.104│
└─────────────┘
```

> The Windows 11 machine is the physical host and endpoint. It is used for management access and endpoint-agent testing, but it is not part of the pfSense internal LAN.

---

## Project Map

| Project | Area | Main Tools | Status | Links |
|---|---|---|---:|---|
| **Project 01** | Firewall and IDS | pfSense, Snort, Kali, Nmap | ✅ Completed | [Open](./project-01-pfsense-snort/) |
| **Project 02** | Vulnerability Management | Nessus, Nmap, Kali | ✅ Completed | [Open](./project-02-vulnerability-management/) |
| **Project 03** | SIEM and Log Monitoring | Wazuh, Syslog, Snort, Windows Logs | ✅ Completed | [Open](./project-03-wazuh-siem/) |
| **Project 04** | Endpoint Management | Action1, Windows 11, Kali Linux | ✅ Completed | [Open](./project-04-endpoint-management-action1/) |

---

## Visual Progress

```text
[████████████████████] Project 01 - pfSense + Snort IDS
[████████████████████] Project 02 - Vulnerability Management
[████████████████████] Project 03 - Wazuh SIEM Monitoring
[████████████████████] Project 04 - Endpoint Management
```

---

## What This Lab Demonstrates

| Capability | Demonstrated Through |
|---|---|
| Network segmentation and lab isolation | pfSense internal lab gateway |
| IDS deployment and validation | Snort on pfSense LAN interface |
| Custom detection logic | Custom Snort rules and controlled traffic |
| Vulnerability discovery | Nessus scans against intentionally vulnerable targets |
| Risk prioritization | Severity review, CVEs, affected services, remediation notes |
| SIEM operations | Wazuh dashboard, agents, alerts, syslog ingestion |
| Endpoint monitoring | Windows logs, Linux activity, FIM events |
| Endpoint management | Action1 onboarding, inventory, patch visibility, remote actions |
| Evidence-based documentation | Screenshots, reports, exported files, structured notes |

---

## Repository Structure

```text
blue-team-homelab/
│
├── README.md
├── .gitignore
│
├── docs/
│   ├── lab-notes.md
│   ├── roadmap.md
│   ├── skills-matrix.md
│   └── tools-used.md
│
├── topology/
│   └── ip-addressing.md
│
├── project-01-pfsense-snort/
│   ├── README.md
│   ├── report.md
│   ├── rules/
│   └── evidence/
│
├── project-02-vulnerability-management/
│   ├── README.md
│   ├── report.md
│   └── evidence/
│
├── project-03-wazuh-siem/
│   ├── README.md
│   ├── report.md
│   ├── notes/
│   └── evidence/
│
└── project-04-endpoint-management-action1/
    ├── README.md
    └── evidence/
```

---

## Documentation Index

| Document | Purpose |
|---|---|
| [Lab Notes](./docs/lab-notes.md) | General design decisions, operational notes, and cross-project lessons |
| [Roadmap](./docs/roadmap.md) | Completed projects, planned improvements, and future project ideas |
| [Tools Used](./docs/tools-used.md) | Overview of tools used across the portfolio |
| [Skills Matrix](./docs/skills-matrix.md) | Skills mapped to projects and technical activities |
| [IP Addressing](./topology/ip-addressing.md) | Network layout, IP plan, and management access notes |

---

## Lab Network Summary

| Network Area | Range | Purpose |
|---|---|---|
| Internal Lab Network | `172.30.2.0/24` | pfSense LAN, Kali, vulnerable targets, Wazuh internal interface |
| Management / Host Access | `192.168.6.0/24` | Windows host access to dashboards and management interfaces |
| Cloud Management | Internet-based | Action1 console and endpoint management platform |

---

## Tools and Technologies

| Category | Tools |
|---|---|
| Virtualization | VirtualBox |
| Firewall / Gateway | pfSense |
| IDS / Network Detection | Snort |
| Vulnerability Management | Nessus Essentials, Nmap |
| SIEM / Log Monitoring | Wazuh |
| Endpoint Management | Action1 |
| Operating Systems | Kali Linux, Windows 11, Metasploitable2, Metasploitable3 |
| Evidence and Documentation | Markdown, screenshots, CSV exports, PDF reports |
| Version Control | Git, GitHub |

---

## Project Highlights

### Project 01 - pfSense + Snort IDS

Configured Snort IDS on pfSense, enabled community rules, created custom detection rules, generated controlled traffic from Kali Linux, and validated alerts through Snort evidence.

**Focus:** Network visibility, IDS validation, custom rule testing.

[View Project 01](./project-01-pfsense-snort/)

---

### Project 02 - Vulnerability Management with Nessus

Performed a basic vulnerability management workflow using Nessus and Nmap against intentionally vulnerable lab machines. Findings were documented with severity, affected services, evidence, and remediation recommendations.

**Focus:** Vulnerability scanning, risk review, remediation planning.

[View Project 02](./project-02-vulnerability-management/)

---

### Project 03 - Wazuh SIEM and Endpoint Log Monitoring

Deployed and validated Wazuh monitoring workflows using Windows and Linux agents, Windows event collection, File Integrity Monitoring, Kali activity, pfSense syslog forwarding, and Snort alert ingestion.

**Focus:** SIEM visibility, endpoint telemetry, alert analysis.

[View Project 03](./project-03-wazuh-siem/)

---

### Project 04 - Endpoint Management with Action1

Used Action1 to validate endpoint onboarding, asset inventory, software inventory, patch visibility, vulnerability visibility, remote actions, reporting, and Linux endpoint validation.

**Focus:** Endpoint management, patch visibility, remote administration.

[View Project 04](./project-04-endpoint-management-action1/)

---

## Blue Team Coverage Map

```text
┌───────────────────────────────┬──────────────────────────────┐
│ Blue Team Area                │ Covered By                   │
├───────────────────────────────┼──────────────────────────────┤
│ Network Security Monitoring   │ Project 01, Project 03       │
│ Intrusion Detection           │ Project 01, Project 03       │
│ Vulnerability Management      │ Project 02, Project 04       │
│ SIEM and Log Analysis         │ Project 03                   │
│ Endpoint Monitoring           │ Project 03                   │
│ Endpoint Management           │ Project 04                   │
│ Patch Visibility              │ Project 04                   │
│ Evidence Collection           │ All Projects                 │
│ Technical Documentation       │ All Projects                 │
└───────────────────────────────┴──────────────────────────────┘
```

---

## How to Navigate This Repository

Start here:

1. Review the [IP addressing and topology](./topology/ip-addressing.md).
2. Open the [skills matrix](./docs/skills-matrix.md) for a quick capability overview.
3. Review each project folder in order.
4. Open each project `README.md` for the summary and `report.md` for deeper technical documentation when available.
5. Use the `evidence/` folders to validate screenshots, exports, reports, and supporting files.

---

## Current Status

| Area | Status |
|---|---:|
| Lab architecture documented | ✅ Done |
| IP addressing documented | ✅ Done |
| Project 01 completed | ✅ Done |
| Project 02 completed | ✅ Done |
| Project 03 completed | ✅ Done |
| Project 04 completed | ✅ Done |
| General docs added | ✅ Done |
| Future improvements planned | 🔄 Ongoing |

---

## Future Improvements

Potential next steps for this portfolio:

- Add GIFs or short screen recordings to improve visual presentation.
- Add a dedicated detection engineering project.
- Add an incident response simulation project.
- Add a Windows hardening baseline project.
- Add a threat intelligence mini project.
- Improve diagrams with exported PNG/SVG topology visuals.
- Add more cross-project dashboards and summary tables.

---

## Disclaimer

This repository documents a private cybersecurity home lab created for educational and portfolio purposes.

All scans, alerts, logs, tests, and endpoint actions were performed in controlled environments using owned systems, intentionally vulnerable machines, or personal endpoints.

No public IP addresses, third-party systems, production networks, or unauthorized targets were scanned or attacked.

---

## Author

**Joao Caetano**  
Blue Team / Defensive Security Portfolio

[LinkedIn](https://www.linkedin.com/in/joaocaetano02/)
