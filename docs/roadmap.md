# Blue Team Home Lab Roadmap

> Project roadmap for the Blue Team home lab portfolio.

---

## Portfolio Progress

```text
Project 01  [██████████] Completed  pfSense + Snort IDS
Project 02  [██████████] Completed  Vulnerability Management with Nessus
Project 03  [██████████] Completed  Wazuh SIEM and Endpoint Log Monitoring
Project 04  [██████████] Completed  Endpoint Management with Action1
Project 05  [░░░░░░░░░░] Planned    Detection Engineering / Incident Response
Project 06  [░░░░░░░░░░] Planned    Windows Hardening / Baseline Validation
```

---

## Completed Projects

| # | Project | Status | Main Focus | Main Tools |
|---:|---|---|---|---|
| 01 | pfSense + Snort IDS | Completed | Firewall, IDS, custom rules, alert validation | pfSense, Snort, Kali, Nmap |
| 02 | Vulnerability Management with Nessus | Completed | Vulnerability scanning, risk prioritization, remediation | Nessus, Nmap, Kali |
| 03 | Wazuh SIEM and Endpoint Log Monitoring | Completed | SIEM, endpoint logs, FIM, syslog, alert analysis | Wazuh, pfSense, Snort, Kali, Windows |
| 04 | Endpoint Management with Action1 | Completed | Endpoint inventory, patch visibility, vulnerability visibility, remote actions | Action1, Windows 11, Kali |

---

## Project Timeline View

```text
┌────────────┐     ┌────────────┐     ┌────────────┐     ┌────────────┐
│ Project 01 │ ──> │ Project 02 │ ──> │ Project 03 │ ──> │ Project 04 │
│ IDS        │     │ Vuln Mgmt  │     │ SIEM       │     │ Endpoint   │
└────────────┘     └────────────┘     └────────────┘     └────────────┘
      │                  │                  │                  │
      ▼                  ▼                  ▼                  ▼
 Network             Exposure            Detection          Endpoint
 Visibility          Visibility          & Triage           Operations
```

---

## Current Coverage Map

| Capability Area | Current Coverage | Evidence Status |
|---|---|---|
| Lab network segmentation | Covered | Documented in topology and project READMEs |
| Firewall/gateway validation | Covered | Project 01 |
| IDS alert validation | Covered | Project 01 and Project 03 |
| Vulnerability scanning | Covered | Project 02 |
| Risk prioritization | Covered | Project 02 |
| SIEM dashboard validation | Covered | Project 03 |
| Windows log collection | Covered | Project 03 |
| Linux event validation | Covered | Project 03 and Project 04 |
| File Integrity Monitoring | Covered | Project 03 |
| Syslog forwarding | Covered | Project 03 |
| Endpoint inventory | Covered | Project 04 |
| Patch visibility | Covered | Project 04 |
| Endpoint-based vulnerability visibility | Covered | Project 04 |
| Remote administrative action | Covered | Project 04 |
| Detection engineering | Planned | Future project |
| Incident response simulation | Planned | Future project |
| Hardening baseline | Planned | Future project |

---

## Recommended Next Improvements

### Repository Presentation

| Priority | Improvement | Reason |
|---:|---|---|
| 1 | Improve root `README.md` with project cards | Helps recruiters quickly understand the portfolio |
| 2 | Add GIFs or short recordings | Makes the project more visual and engaging |
| 3 | Add a skills matrix | Connects technical skills to project evidence |
| 4 | Add a better topology diagram | Makes the lab architecture easier to understand |
| 5 | Add links between docs and projects | Improves navigation across the repository |

---

## Possible Future Projects

| Project Idea | Focus | Possible Evidence |
|---|---|---|
| Project 05 - Detection Engineering | Create and validate basic detection logic | Sigma rules, Wazuh local rules, alert screenshots |
| Project 06 - Incident Response Simulation | Investigate a simulated security event | Timeline, triage notes, containment notes |
| Project 07 - Windows Hardening Baseline | Review and apply basic Windows security settings | Screenshots, audit policy, hardening checklist |
| Project 08 - Threat Intelligence Mini Project | Analyze indicators and build a small report | IOC table, enrichment notes, summary report |
| Project 09 - Dashboard and Metrics | Build a visual security operations summary | Charts, screenshots, Markdown report |

---

## Future Project Ideas - Visual Flow

```text
Detection Engineering
        │
        ▼
Incident Response Simulation
        │
        ▼
Windows Hardening Baseline
        │
        ▼
Threat Intelligence Mini Project
        │
        ▼
Portfolio Polish: GIFs, diagrams, project cards
```

---

## Suggested Project 05 Direction

A strong next project would be:

```text
Project 05 - Detection Engineering and Alert Triage
```

Possible scope:

- Create custom detection rules.
- Generate controlled events.
- Validate alerts in Wazuh.
- Document rule logic.
- Export JSON evidence.
- Write analyst-style triage notes.

Why this is a good next step:

| Reason | Benefit |
|---|---|
| Builds on Wazuh from Project 03 | Reuses existing SIEM setup |
| Builds on Snort from Project 01 | Connects network detection with SIEM analysis |
| Shows analyst thinking | Strong for SOC / Blue Team interviews |
| Creates readable evidence | Easy to show in GitHub/GitLab and LinkedIn |

---

## Long-Term Portfolio Goal

The long-term goal is to turn this repository into a clean Blue Team portfolio that demonstrates:

```text
Network Security  +  Vulnerability Management  +  SIEM  +  Endpoint Operations
        │                       │                  │             │
        └───────────────────────┴──────────────────┴─────────────┘
                                │
                                ▼
                    Practical Blue Team Capability
```

---

## Status

Active roadmap.
