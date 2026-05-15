# Lab Notes

> General design notes, scope decisions, operational observations, and lessons learned across the Blue Team home lab portfolio.

---

## Lab Purpose

This Blue Team home lab was built to demonstrate practical defensive security skills through hands-on projects, structured documentation, and evidence-based validation.

The goal is not to simulate a full enterprise environment. Instead, the lab provides a controlled and repeatable environment to practice key security operations concepts such as:

| Area | Practical Focus |
|---|---|
| Network Security Monitoring | Firewalling, IDS validation, controlled traffic generation |
| Vulnerability Management | Asset scanning, service discovery, severity review, remediation planning |
| SIEM Operations | Log collection, alert review, event triage, endpoint telemetry |
| Endpoint Monitoring | Agent deployment, Windows events, Linux activity, FIM validation |
| Endpoint Management | Inventory, patch visibility, vulnerability visibility, remote actions |
| Documentation | Evidence collection, reports, screenshots, structured Markdown |

---

## High-Level Lab Layout

```text
                         ┌──────────────────────────────┐
                         │      Windows 11 Host          │
                         │  Physical host / workstation  │
                         │  Dashboard access / Action1   │
                         └───────────────┬──────────────┘
                                         │
                         Management / Host Access Network
                                  192.168.6.0/24
                                         │
              ┌──────────────────────────┴──────────────────────────┐
              │                                                     │
              │                                                     │
┌─────────────▼─────────────┐                         ┌─────────────▼─────────────┐
│       Wazuh Dashboard      │                         │      Action1 Console       │
│ Host-side dashboard access │                         │    Cloud-based platform    │
└─────────────┬─────────────┘                         └───────────────────────────┘
              │
              │
              │ Internal Lab Network
              │ 172.30.2.0/24
              │
        ┌─────▼─────┐
        │  pfSense  │
        │ Gateway   │
        │ Firewall  │
        │ Snort IDS │
        └─────┬─────┘
              │
   ┌──────────┼──────────┬───────────────┬───────────────┐
   │          │          │               │               │
┌──▼───┐  ┌───▼────┐ ┌───▼────┐    ┌─────▼─────┐   ┌─────▼─────┐
│ Kali │  │ MSF2   │ │ MSF3   │    │ Wazuh     │   │ Future    │
│ .100 │  │ .101   │ │ .102   │    │ Server    │   │ Lab VMs   │
└──────┘  └────────┘ └────────┘    │ .104      │   └───────────┘
                                   └───────────┘
```

---

## Core Design Decisions

| Decision | Reason |
|---|---|
| Use pfSense as the internal gateway | Creates a realistic firewall/gateway layer for the lab |
| Use an isolated private subnet | Keeps vulnerable machines away from public or production networks |
| Use Kali Linux as the main testing system | Centralizes scanning, validation, and controlled traffic generation |
| Use intentionally vulnerable machines | Allows safe vulnerability scanning and IDS validation |
| Use Wazuh for SIEM workflows | Provides endpoint telemetry, log analysis, FIM, and alert review |
| Use Action1 separately for endpoint management | Demonstrates inventory, patch visibility, vulnerability visibility, and remote actions |
| Keep each project scoped separately | Prevents tool mixing and keeps documentation clear |

---

## Network Notes

| Network Area | Addressing | Purpose |
|---|---:|---|
| Internal Lab LAN | `172.30.2.0/24` | Virtual lab traffic between pfSense, Kali, vulnerable machines, and Wazuh |
| Management / Host Access | `192.168.6.0/24` | Host-side access to dashboards and management interfaces |
| Cloud Console Access | Internet-based | Used for Action1 cloud console access from the Windows 11 host |

### Important Placement Note

The **Windows 11 endpoint is the physical host machine** and is **outside the pfSense internal LAN**.

It is used for:

- Accessing web dashboards.
- Managing the repository.
- Running the Action1 agent for Project 04.
- Acting as a practical Windows endpoint for agent-based validation.

It should not be treated as an internal `172.30.2.x` pfSense LAN endpoint.

---

## Project Scope Notes

| Project | Main Scope | Important Scope Boundary |
|---|---|---|
| Project 01 - pfSense + Snort IDS | IDS deployment and alert validation | Focused on Snort, not full firewall hardening |
| Project 02 - Vulnerability Management | Nessus scanning and vulnerability analysis | Wazuh was powered off and out of scope |
| Project 03 - Wazuh SIEM | Log collection, endpoint monitoring, alert analysis | Not focused on patch management or EDR replacement |
| Project 04 - Endpoint Management | Action1 endpoint visibility and remote actions | Not a full enterprise endpoint management deployment |

---

## Operational Notes

### Evidence Collection

Evidence is collected using screenshots, exported reports, logs, and Markdown documentation.

```text
project-xx-name/
├── README.md
├── report.md
└── evidence/
    ├── screenshots/
    ├── exported-reports/
    └── notes-or-json-exports/
```

Evidence naming follows a numbered format when possible:

```text
01-description.png
02-description.png
03-description.png
```

This makes the project easier to review in chronological order.

---

### Tool Usage Strategy

| Tool | Usage Pattern |
|---|---|
| Snort | Used for IDS validation and alert generation |
| Nessus | Used for vulnerability scanning and risk review |
| Wazuh | Used for log collection, SIEM analysis, and endpoint telemetry |
| Action1 | Used for endpoint inventory, patch visibility, vulnerability visibility, and remote actions |
| Nmap | Used for service discovery and controlled traffic generation |
| tcpdump | Used to validate syslog and network traffic flow |
| PowerShell | Used for Windows endpoint validation |
| Bash | Used for Linux validation and command-line evidence |

---

## Lessons Learned Across the Lab

| Lesson | Why It Matters |
|---|---|
| Scope control is critical | Each project becomes clearer when only the relevant tools are included |
| Evidence quality matters | Screenshots, reports, and exports make the work easier to verify |
| Visibility is layered | Network, endpoint, vulnerability, and SIEM visibility solve different problems |
| Documentation is part of the skill | A clear report shows both technical ability and communication ability |
| Simple tests are valuable | Ping, Nmap, service checks, logs, and screenshots can validate a full workflow |
| Tool integration needs validation | tcpdump and local logs help confirm whether integrations are actually working |

---

## Portfolio Direction

Future improvements should focus on making the repository easier to review visually:

- Add GIFs or short recordings for selected workflows.
- Add project cards to the main README.
- Add a visual architecture diagram.
- Add links between topology, tools, roadmap, and each project.
- Keep evidence organized and consistent.

---

## Status

Active and evolving.
