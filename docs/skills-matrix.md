# Skills Matrix

> Mapping of practical Blue Team skills demonstrated across the home lab portfolio.

---

## Executive Summary

This matrix connects technical skills to the projects where they were demonstrated.

```text
Network Security      ─┐
Vulnerability Mgmt    ─┼──> Blue Team Home Lab Portfolio
SIEM Operations       ─┤
Endpoint Monitoring   ─┤
Endpoint Management   ─┘
```

---

## Skills Coverage Heatmap

| Skill Area | P01 IDS | P02 Vuln Mgmt | P03 SIEM | P04 Endpoint Mgmt |
|---|:---:|:---:|:---:|:---:|
| Network fundamentals | ✅ | ✅ | ✅ | ◐ |
| Firewall/gateway concepts | ✅ | ✅ | ✅ | ◐ |
| IDS deployment | ✅ | — | ✅ | — |
| Custom detection validation | ✅ | — | ✅ | — |
| Vulnerability scanning | — | ✅ | — | ◐ |
| Risk prioritization | — | ✅ | ◐ | ◐ |
| SIEM operations | — | — | ✅ | — |
| Endpoint log collection | — | — | ✅ | — |
| File Integrity Monitoring | — | — | ✅ | — |
| Syslog forwarding | — | — | ✅ | — |
| Alert triage | ◐ | ◐ | ✅ | ◐ |
| Endpoint inventory | — | — | ◐ | ✅ |
| Patch visibility | — | — | — | ✅ |
| Remote actions | — | — | — | ✅ |
| Documentation | ✅ | ✅ | ✅ | ✅ |
| Evidence collection | ✅ | ✅ | ✅ | ✅ |

Legend:

| Symbol | Meaning |
|---|---|
| ✅ | Strongly demonstrated |
| ◐ | Partially demonstrated or indirectly related |
| — | Not covered in that project |

---

## Detailed Skill Mapping

| Skill Area | Demonstrated Through | Evidence Location |
|---|---|---|
| Network Security Monitoring | pfSense gateway, Snort IDS on LAN, controlled traffic generation | `project-01-pfsense-snort/` |
| IDS Rule Validation | Snort GPLv2 rules, custom Snort rules, alert review | `project-01-pfsense-snort/rules/` and `evidence/` |
| Vulnerability Management | Nessus scans, Nmap pre-scan validation, severity review, remediation notes | `project-02-vulnerability-management/` |
| Service Discovery | Nmap scans against vulnerable targets | Project 01 and Project 02 evidence |
| SIEM Operations | Wazuh dashboard validation, agent deployment, log analysis | `project-03-wazuh-siem/` |
| Windows Event Monitoring | Windows authentication, failed logon, service/application events | Project 03 Windows log collection evidence |
| Linux Activity Monitoring | Kali agent validation, sudo activity, journald review | Project 03 controlled events evidence |
| File Integrity Monitoring | Create, modify, and delete events in a monitored Windows folder | Project 03 FIM evidence |
| Syslog Integration | pfSense remote syslog forwarding to Wazuh | Project 03 Snort integration evidence |
| Alert Analysis | Exported JSON alerts and analyst-style triage notes | Project 03 alert analysis evidence |
| Endpoint Management | Action1 onboarding, inventory, software visibility, patch visibility | `project-04-endpoint-management-action1/` |
| Remote Administration | Safe read-only script execution through Action1 | Project 04 automation and remote actions evidence |
| Reporting | Nessus PDFs, Action1 reports, Markdown reports | Project 02 and Project 04 evidence |
| Technical Documentation | README files, report files, evidence folders, topology docs | Entire repository |

---

## Role-Relevant Skill Groups

### SOC Analyst / Blue Team Analyst

| Skill | Portfolio Evidence |
|---|---|
| Alert review | Snort alerts and Wazuh alerts |
| Log analysis | Windows events, Kali events, pfSense syslog |
| Basic triage | Project 03 alert analysis notes |
| Detection validation | Controlled traffic and custom rules |
| Documentation | Reports and evidence-based writeups |

---

### Vulnerability Management / Security Operations

| Skill | Portfolio Evidence |
|---|---|
| Asset identification | Lab environment tables and topology docs |
| Pre-scan validation | Ping and Nmap evidence |
| Vulnerability scanning | Nessus Basic Network Scans |
| Risk prioritization | Critical/High/Medium findings tables |
| Remediation thinking | Project 02 reporting and recommendations |
| Reporting | Nessus PDF exports and Markdown documentation |

---

### Endpoint / IT Security Operations

| Skill | Portfolio Evidence |
|---|---|
| Agent deployment | Wazuh agent and Action1 agent validation |
| Endpoint inventory | Action1 asset inventory |
| Software inventory | Action1 installed software review |
| Patch visibility | Action1 update approval and patch views |
| Remote action validation | Safe Action1 read-only script |
| Windows administration | PowerShell validation commands |
| Linux validation | Kali Linux system validation commands |

---

## Project-by-Project Skill Summary

### Project 01 - pfSense + Snort IDS

```text
Focus: Network visibility and IDS validation

Skills:
├── pfSense LAN monitoring
├── Snort IDS configuration
├── Community rules activation
├── Custom Snort rule writing
├── Controlled traffic generation
├── Alert validation
└── CSV evidence export
```

---

### Project 02 - Vulnerability Management with Nessus

```text
Focus: Vulnerability discovery and risk review

Skills:
├── Asset scoping
├── Connectivity validation
├── Nmap service discovery
├── Nessus scan configuration
├── Vulnerability result review
├── CVE/severity analysis
├── Risk prioritization
└── Remediation-oriented reporting
```

---

### Project 03 - Wazuh SIEM and Endpoint Log Monitoring

```text
Focus: SIEM, endpoint telemetry, and alert analysis

Skills:
├── Wazuh server validation
├── Dashboard access validation
├── Windows agent monitoring
├── Kali Linux agent monitoring
├── Windows event collection
├── File Integrity Monitoring
├── pfSense syslog forwarding
├── Snort alert ingestion
├── Custom local rule validation
└── Analyst-style alert analysis
```

---

### Project 04 - Endpoint Management with Action1

```text
Focus: Endpoint management concepts

Skills:
├── Action1 console navigation
├── Windows endpoint onboarding
├── Local agent service validation
├── Asset inventory review
├── Software inventory review
├── Patch visibility review
├── Vulnerability visibility review
├── Safe remote script execution
├── Built-in reporting review
└── Linux endpoint visibility validation
```

---

## Capability Maturity View

| Capability | Current Level | Notes |
|---|---|---|
| Network visibility | Strong foundation | pfSense, Snort, Nmap, controlled traffic |
| Vulnerability management | Strong foundation | Nessus workflow and reporting completed |
| SIEM operations | Strong foundation | Wazuh agents, FIM, syslog, alert analysis completed |
| Endpoint management | Introductory practical coverage | Action1 used for inventory, patch, vulnerability, and remote actions |
| Detection engineering | Early stage | Custom Snort and Wazuh local rules introduced |
| Incident response | Planned | Future project can add timeline and containment workflow |
| Hardening and baseline management | Planned | Future project can focus on Windows/Linux baseline validation |

---

## Interview Talking Points

| Topic | How to Explain It |
|---|---|
| Lab purpose | Built a structured Blue Team home lab to practice defensive security workflows |
| Evidence approach | Each project includes screenshots, reports, exports, and Markdown documentation |
| Scope control | Each project has clear in-scope and out-of-scope boundaries |
| Tool selection | Used different tools for network, vulnerability, SIEM, and endpoint visibility |
| Practical validation | Did not only install tools; validated functionality with controlled events |
| Analyst mindset | Included alert review, risk prioritization, and remediation-oriented notes |

---

## Overall Skill Graph

```text
Documentation        [██████████] Strong
Evidence Collection  [██████████] Strong
Network Security     [████████░░] Strong foundation
Vuln Management      [████████░░] Strong foundation
SIEM Operations      [████████░░] Strong foundation
Endpoint Management  [███████░░░] Practical introduction
Detection Logic      [█████░░░░░] Started
Incident Response    [██░░░░░░░░] Planned
Hardening Baseline   [██░░░░░░░░] Planned
```

---

## Status

Active skill matrix.
