# Project 03 Report - Wazuh SIEM and Endpoint Log Monitoring

## Executive Summary

This project implemented and validated a basic SIEM and log monitoring workflow using Wazuh in an isolated Blue Team home lab.

The project focused on endpoint telemetry, Windows log collection, File Integrity Monitoring, controlled Kali Linux activity, pfSense syslog forwarding, and Snort IDS alert ingestion into Wazuh.

The final result demonstrates a practical monitoring workflow where multiple telemetry sources are collected and reviewed in a central SIEM platform.

---

## Objectives

The objectives of this project were to:

- Validate the Wazuh Server and Dashboard.
- Validate Wazuh Agent communication from monitored endpoints.
- Collect Windows security and service/application logs.
- Configure and test File Integrity Monitoring on Windows.
- Use Kali Linux as a monitored endpoint and controlled event generator.
- Forward pfSense syslog data to Wazuh.
- Forward Snort IDS alerts from pfSense to Wazuh using syslog.
- Create local Wazuh rules for lab validation.
- Analyze selected alerts using JSON evidence and basic SOC triage logic.

---

## Lab Environment

| Asset | Role | IP / Identifier |
|---|---|---|
| pfSense | Gateway / Firewall / Snort IDS | `172.30.2.1` |
| Kali Linux | Monitored endpoint and controlled event generator | `172.30.2.100` / Agent: `Kali` |
| Metasploitable2 | Vulnerable lab target | `172.30.2.101` |
| Metasploitable3 | Optional vulnerable target | `172.30.2.102` |
| Wazuh Server | SIEM / Log collection / Dashboard | `172.30.2.104` |
| Windows 11 Endpoint | Primary monitored endpoint | Agent: `Win11JOAO` |

All testing was conducted inside an isolated virtual lab using private IP addressing.

---

## Scope

### In Scope

- Wazuh service validation.
- Wazuh dashboard access.
- Windows and Kali agent validation.
- Windows authentication log collection.
- Windows FIM testing.
- Kali sudo and Nmap-controlled event generation.
- pfSense syslog forwarding to Wazuh.
- Snort alert forwarding through pfSense syslog.
- Basic alert analysis.

### Out of Scope

- Production deployment.
- Enterprise-grade rule tuning.
- Automated incident response.
- Endpoint hardening and patch management.
- Full custom decoder development for pfSense/Snort.
- Advanced threat hunting content.

---

## Methodology

The project was executed in eight phases:

1. Wazuh Server validation.
2. Dashboard access validation.
3. Agent deployment and validation.
4. Windows log collection.
5. File Integrity Monitoring on Windows.
6. Controlled events with Kali.
7. pfSense/Snort syslog integration.
8. Alert analysis.

Each phase produced screenshots, JSON exports, or notes stored under the `evidence/` and `notes/` directories.

---

## Phase 1 - Wazuh Server Validation

The Wazuh Server was validated by confirming:

- IP addressing.
- Routing table.
- Wazuh Manager service status.
- Wazuh Indexer service status.
- Wazuh Dashboard service status.
- Listening ports.

Evidence:

- `evidence/server-validation/01-wazuh-server-ip-address.png`
- `evidence/server-validation/02-wazuh-server-routing-table.png`
- `evidence/server-validation/03-wazuh-manager-service-status.png`
- `evidence/server-validation/04-wazuh-indexer-service-status.png`
- `evidence/server-validation/05-wazuh-dashboard-service-status.png`
- `evidence/server-validation/06-wazuh-listening-ports.png`

### Result

The Wazuh Server was confirmed as operational and reachable inside the lab network.

---

## Phase 2 - Dashboard Access

The Wazuh Dashboard was accessed from the Windows host and validated through the login page and dashboard overview.

Evidence:

- `evidence/dashboard-access/01-wazuh-dashboard-login-page.png`
- `evidence/dashboard-access/02-wazuh-dashboard-overview.png`

### Result

The Wazuh Dashboard was accessible and ready for monitoring and event review.

---

## Phase 3 - Agent Deployment and Validation

The Windows 11 endpoint was selected as the primary monitored endpoint. The Wazuh deployment workflow was documented, and the existing Windows agent was validated instead of being removed and reinstalled.

The validation confirmed:

- The agent deployment command was available from the Wazuh Dashboard.
- The Windows Wazuh Agent service was running.
- The Windows endpoint appeared in the dashboard.
- The agent was active and reporting to Wazuh.

Evidence:

- `evidence/agent-deployment/01-deploy-new-agent-windows-config.png`
- `evidence/agent-deployment/02-windows-agent-install-command.png`
- `evidence/agent-deployment/03-windows-agent-service-running.png`
- `evidence/agent-deployment/04-windows-agent-visible-in-dashboard.png`
- `evidence/agent-deployment/05-windows-agent-active-in-dashboard.png`

### Result

The Windows 11 endpoint was confirmed as an active monitored endpoint.

---

## Phase 4 - Windows Log Collection

Windows log collection was validated using authentication and service/application events.

Collected event types included:

- Windows authentication success.
- Windows failed logon.
- Windows service/application event.

The failed logon event was later exported as JSON and analyzed in the alert analysis phase.

Evidence:

- `evidence/windows-log-collection/01-windows-agent-security-events.png`
- `evidence/windows-log-collection/02-windows-authentication-event.png`
- `evidence/windows-log-collection/03-windows-failed-logon-event.png`
- `evidence/windows-log-collection/04-windows-failed-logon-event.png`
- `evidence/windows-log-collection/05-windows-failed-logon-event.png`
- `evidence/windows-log-collection/06-windows-service-event.png`

### Result

Wazuh successfully collected Windows security and service/application events from the monitored Windows endpoint.

---

## Phase 5 - File Integrity Monitoring on Windows

A controlled directory was created on the Windows endpoint:

```text
C:\Wazuh-FIM-Lab
```

The Wazuh Agent was configured to monitor this directory using FIM. A test file was created, modified, and deleted to validate file integrity alerts.

Evidence:

- `evidence/fim-monitoring/01-fim-folder-created.png`
- `evidence/fim-monitoring/02-windows-agent-fim-config.png`
- `evidence/fim-monitoring/03-fim-file-created-alert.png`
- `evidence/fim-monitoring/04-fim-file-modified-alert.png`
- `evidence/fim-monitoring/05-fim-file-deleted-alert.png`

### Result

Wazuh detected file creation, modification, and deletion events in the monitored Windows directory.

---

## Phase 6 - Controlled Events with Kali

Kali Linux was used as both a monitored endpoint and controlled event generator.

Activities performed:

```bash
ping 172.30.2.101
sudo nmap -sS 172.30.2.101
sudo nmap -A 172.30.2.101
```

The Kali Wazuh Agent also captured sudo activity from journald, including the Nmap command executed with elevated privileges.

Evidence:

- `evidence/controlled-events/01-kali-agent-active-in-dashboard.png`
- `evidence/controlled-events/02-kali-ping-metasploitable2.png`
- `evidence/controlled-events/03-kali-nmap-syn-scan-metasploitable2.png`
- `evidence/controlled-events/04-kali-nmap-aggressive-scan-metasploitable2.png`
- `evidence/controlled-events/05-wazuh-kali-sudo-events.png`
- `evidence/controlled-events/06-kali-journalctl-sudo-nmap-command.png`

### Result

Kali generated controlled network activity against Metasploitable2 and also provided endpoint telemetry through its Wazuh Agent.

---

## Phase 7 - pfSense/Snort Integration with Wazuh

pfSense was configured to forward remote syslog messages to the Wazuh Server:

```text
pfSense 172.30.2.1 -> Syslog UDP/514 -> Wazuh Server 172.30.2.104
```

A Wazuh `<remote>` listener was configured for syslog ingestion. Remote syslog was enabled on pfSense. Snort was configured to send alerts to the pfSense system log, allowing alerts to be forwarded to Wazuh through pfSense syslog.

Custom local Wazuh rules were used to validate pfSense and Snort syslog messages.

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

### Result

The integration successfully demonstrated pfSense syslog forwarding and Snort IDS alert ingestion into Wazuh.

---

## Phase 8 - Alert Analysis

Selected events were exported as JSON and reviewed from a basic SOC/SIEM triage perspective.

Analyzed events:

| Alert Category | Source | Rule ID | Level |
|---|---|---:|---:|
| Windows failed logon | Windows Event Channel | `60122` | `5` |
| FIM file modification | Wazuh Syscheck | `550` | `7` |
| Kali sudo activity | journald / sudo | `5402` | `3` |
| pfSense syslog event | pfSense via syslog | `100301` | `3` |
| Snort IDS alert | Snort via pfSense syslog | `100310` | `7` |

Evidence:

- `evidence/alert-analysis/alert-analysis.md`
- `evidence/alert-analysis/windows-failed-logon-event.json`
- `evidence/alert-analysis/fim-file-modification-event.json`
- `evidence/alert-analysis/kali-sudo-event.json`
- `evidence/alert-analysis/pfsense-syslog-event.json`
- `evidence/alert-analysis/snort-ids-alert-event.json`

### Result

The alert analysis phase demonstrated how rule metadata, severity, source context, raw logs, and analyst interpretation can be used to document SIEM events.

---

## Findings

### Finding 1 - Endpoint Visibility

Wazuh agents successfully collected endpoint telemetry from Windows 11 and Kali Linux.

### Finding 2 - File Integrity Monitoring

FIM detected content changes to a monitored Windows file and provided hash-based evidence of the modification.

### Finding 3 - Linux Administrative Activity

Kali sudo activity was captured by Wazuh from journald, including the Nmap command used during controlled testing.

### Finding 4 - Network Device Log Ingestion

pfSense logs were successfully forwarded to Wazuh using syslog over UDP port `514`.

### Finding 5 - IDS Alert Ingestion

Snort generated an IDS alert for controlled Kali-to-Metasploitable2 traffic, and the alert was forwarded to Wazuh through pfSense syslog.

---

## Lessons Learned

- A working SIEM lab should validate both endpoint and network telemetry.
- Windows agents are useful for authentication events and FIM.
- Linux agents can provide visibility into sudo activity and administrative command execution.
- pfSense remote syslog requires both Wazuh listener configuration and pfSense remote logging configuration.
- Snort alerts required enabling `Send Alerts to System Log` before they could be forwarded through pfSense syslog.
- `tcpdump` was essential for validating whether syslog traffic reached the Wazuh Server before troubleshooting the dashboard.
- Custom Wazuh rules can be used to validate lab-specific syslog ingestion.

---

## Conclusion

This project successfully implemented a practical SIEM monitoring workflow using Wazuh.

The lab validated endpoint agents, Windows log collection, File Integrity Monitoring, Kali Linux sudo telemetry, pfSense syslog forwarding, and Snort IDS alert ingestion.

The project also included JSON-based alert analysis to document rule metadata, severity, telemetry source, and expected analyst response. This demonstrates a foundational SOC workflow suitable for a Blue Team portfolio.

All testing was performed in an isolated lab environment using controlled activity and owned virtual machines.
