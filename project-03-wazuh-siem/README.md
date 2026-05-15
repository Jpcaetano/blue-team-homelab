# Project 03 - Wazuh SIEM and Endpoint Log Monitoring

## Overview

This project documents a basic SIEM and log monitoring workflow using Wazuh in an isolated Blue Team home lab.

The goal was not only to access the Wazuh dashboard, but to validate a practical monitoring process involving endpoint agents, Windows log collection, File Integrity Monitoring (FIM), controlled Linux activity, pfSense syslog forwarding, and Snort IDS alert ingestion.

This project is part of a larger Blue Team Home Lab portfolio.

---

## Lab Scope

### In Scope

- Validate the Wazuh Server and Dashboard.
- Validate Windows 11 Wazuh Agent deployment and communication.
- Collect Windows security and application events.
- Configure File Integrity Monitoring on a controlled Windows directory.
- Use Kali Linux as both a monitored endpoint and controlled event generator.
- Forward pfSense syslog messages to Wazuh.
- Forward Snort IDS alerts from pfSense to Wazuh through syslog.
- Create local Wazuh rules for pfSense/Snort lab validation.
- Analyze selected alerts from a basic SOC/SIEM triage perspective.

### Out of Scope

- Production hardening.
- Full endpoint management lifecycle.
- Patch management.
- Advanced Windows security baselines.
- Automated response actions.
- Enterprise-grade rule tuning.
- Full pfSense/Snort decoder development.

Those topics may be expanded in future projects.

---

## Lab Environment

| Asset | Role | IP / Identifier |
|---|---|---|
| pfSense | Gateway / Firewall / Snort IDS | `172.30.2.1` |
| Kali Linux | Monitored endpoint and controlled event generator | `172.30.2.100` / Agent: `Kali` |
| Metasploitable2 | Vulnerable lab target | `172.30.2.101` |
| Metasploitable3 | Optional vulnerable Windows target | `172.30.2.102` |
| Wazuh Server | SIEM / Log collection / Dashboard | `172.30.2.104` |
| Windows 11 Endpoint | Primary monitored endpoint | Agent: `Win11JOAO` |

> All testing was performed in an isolated virtual lab using owned virtual machines and private IP addressing.

---

## High-Level Architecture

```text
Windows 11 Endpoint ── Wazuh Agent ──┐
                                     │
Kali Linux ─────────── Wazuh Agent ──┼── Wazuh Server / Dashboard
                                     │
pfSense + Snort ───── Syslog UDP 514 ┘

Kali Linux ── controlled traffic ──> Metasploitable2
```

---

## Project Structure

```text
project-03-wazuh-siem/
├── README.md
├── report.md
├── evidence/
│   ├── agent-deployment/
│   ├── alert-analysis/
│   ├── controlled-events/
│   ├── dashboard-access/
│   ├── fim-monitoring/
│   ├── server-validation/
│   ├── snort-integration/
│   └── windows-log-collection/
└── notes/
    └── siem-notes.md
```

---

## Phases Completed

### 1. Wazuh Server Validation

The Wazuh Server was validated by checking network configuration, routing, service status, and listening ports.

Evidence:

- `evidence/server-validation/01-wazuh-server-ip-address.png`
- `evidence/server-validation/02-wazuh-server-routing-table.png`
- `evidence/server-validation/03-wazuh-manager-service-status.png`
- `evidence/server-validation/04-wazuh-indexer-service-status.png`
- `evidence/server-validation/05-wazuh-dashboard-service-status.png`
- `evidence/server-validation/06-wazuh-listening-ports.png`

---

### 2. Dashboard Access

The Wazuh Dashboard was accessed from the Windows host and validated through the login page and overview dashboard.

Evidence:

- `evidence/dashboard-access/01-wazuh-dashboard-login-page.png`
- `evidence/dashboard-access/02-wazuh-dashboard-overview.png`

---

### 3. Agent Deployment and Validation

The Windows 11 endpoint was used as the primary monitored endpoint. The Wazuh deployment workflow was documented, and the existing Windows Agent was validated as active in the dashboard.

Evidence:

- `evidence/agent-deployment/01-deploy-new-agent-windows-config.png`
- `evidence/agent-deployment/02-windows-agent-install-command.png`
- `evidence/agent-deployment/03-windows-agent-service-running.png`
- `evidence/agent-deployment/04-windows-agent-visible-in-dashboard.png`
- `evidence/agent-deployment/05-windows-agent-active-in-dashboard.png`

---

### 4. Windows Log Collection

Windows authentication and service/application events were collected from the Windows 11 endpoint.

Collected event examples:

- Windows authentication success.
- Windows failed logon.
- Windows service/application event.

Evidence:

- `evidence/windows-log-collection/01-windows-agent-security-events.png`
- `evidence/windows-log-collection/02-windows-authentication-event.png`
- `evidence/windows-log-collection/03-windows-failed-logon-event.png`
- `evidence/windows-log-collection/04-windows-failed-logon-event.png`
- `evidence/windows-log-collection/05-windows-failed-logon-event.png`
- `evidence/windows-log-collection/06-windows-service-event.png`

---

### 5. File Integrity Monitoring on Windows

Wazuh FIM was configured to monitor a controlled Windows directory:

```text
C:\Wazuh-FIM-Lab
```

The test file `test-file.txt` was created, modified, and deleted to validate FIM alerting.

Evidence:

- `evidence/fim-monitoring/01-fim-folder-created.png`
- `evidence/fim-monitoring/02-windows-agent-fim-config.png`
- `evidence/fim-monitoring/03-fim-file-created-alert.png`
- `evidence/fim-monitoring/04-fim-file-modified-alert.png`
- `evidence/fim-monitoring/05-fim-file-deleted-alert.png`

---

### 6. Controlled Events with Kali

Kali Linux was used as both a monitored endpoint and a controlled event generator.

Activities performed:

- ICMP test to Metasploitable2.
- Nmap SYN scan against Metasploitable2.
- Nmap aggressive scan against Metasploitable2.
- Sudo activity validation on Kali.
- Local journald validation for sudo command execution.

Evidence:

- `evidence/controlled-events/01-kali-agent-active-in-dashboard.png`
- `evidence/controlled-events/02-kali-ping-metasploitable2.png`
- `evidence/controlled-events/03-kali-nmap-syn-scan-metasploitable2.png`
- `evidence/controlled-events/04-kali-nmap-aggressive-scan-metasploitable2.png`
- `evidence/controlled-events/05-wazuh-kali-sudo-events.png`
- `evidence/controlled-events/06-kali-journalctl-sudo-nmap-command.png`

---

### 7. pfSense and Snort Integration with Wazuh

pfSense was configured to forward syslog messages to the Wazuh Server over UDP port `514`.

Snort was configured to send alerts to the pfSense system log. Those alerts were then forwarded to Wazuh through pfSense remote syslog.

Custom local Wazuh rules were created to validate pfSense and Snort syslog ingestion.

Evidence:

- `evidence/snort-integration/01-wazuh-syslog-config.png`
- `evidence/snort-integration/02-wazuh-syslog-port-listening.png`
- `evidence/snort-integration/03-pfsense-remote-syslog-config.png`
- `evidence/snort-integration/04-pfsense-syslog-packets-tcpdump.png`
- `evidence/snort-integration/05-pfsense-syslog-payload-tcpdump.png`
- `evidence/snort-integration/06-wazuh-local-rule-pfsense-syslog.png`
- `evidence/snort-integration/07-pfsense-syslog-received-by-wazuh.png`
- `evidence/snort-integration/08-snort-alert-generated.png`
- `evidence/snort-integration/09-snort-syslog-payload-tcpdump.png`
- `evidence/snort-integration/10-wazuh-local-rule-snort-syslog.png`
- `evidence/snort-integration/11-snort-alert-received-by-wazuh.png`

---

### 8. Alert Analysis

Selected alerts were exported as JSON and analyzed from a basic SOC/SIEM triage perspective.

Analyzed alert categories:

- Windows failed logon.
- FIM file modification.
- Kali sudo activity.
- pfSense syslog event.
- Snort IDS alert forwarded through pfSense syslog.

Evidence:

- `evidence/alert-analysis/alert-analysis.md`
- `evidence/alert-analysis/windows-failed-logon-event.json`
- `evidence/alert-analysis/fim-file-modification-event.json`
- `evidence/alert-analysis/kali-sudo-event.json`
- `evidence/alert-analysis/pfsense-syslog-event.json`
- `evidence/alert-analysis/snort-ids-alert-event.json`

---

## Key Results

- Wazuh Server and Dashboard were validated.
- Windows 11 and Kali Linux agents were active and sending telemetry.
- Windows authentication events were collected.
- Windows FIM detected file creation, modification, and deletion activity.
- Kali sudo activity was collected from journald.
- pfSense syslog forwarding to Wazuh was validated with `tcpdump`.
- Snort IDS alerts were forwarded from pfSense to Wazuh via syslog.
- Custom Wazuh local rules were used to promote pfSense and Snort syslog messages into searchable events.
- Selected alerts were analyzed with JSON evidence and basic analyst triage notes.

---

## Lessons Learned

- Endpoint telemetry and network telemetry provide different visibility.
- Wazuh agents are useful for host-based events such as Windows authentication, FIM, and Linux sudo activity.
- pfSense/Snort integration required both pfSense remote syslog and Snort alert forwarding to the pfSense system log.
- `tcpdump` was useful for confirming whether syslog packets were actually reaching the Wazuh Server.
- Custom Wazuh rules can be used to validate lab-specific syslog ingestion and alerting.
- Alert analysis should include rule metadata, source, severity, raw log context, and expected analyst action.

---

## Disclaimer

This project was performed in a private, isolated home lab environment. All traffic, scans, alerts, and monitored assets were generated using owned virtual machines and controlled test activity.
