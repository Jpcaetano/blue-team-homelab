# Project 01 - pfSense + Snort IDS Report

## 1. Executive Summary

This report documents the deployment and validation of Snort IDS on a pfSense firewall inside an isolated Blue Team home lab environment.

The objective of this project was to configure Snort on the pfSense LAN interface, enable community and custom detection rules, generate controlled traffic from Kali Linux, and validate that Snort generated alerts.

This project demonstrates a basic network intrusion detection workflow, including network validation, IDS configuration, custom rule creation, controlled traffic generation, alert review, and evidence collection.

This assessment was performed for educational and portfolio purposes only.

---

## 2. Scope

### In Scope

| Asset | IP Address | Description |
|---|---|---|
| pfSense | 172.30.2.1 | Firewall, gateway, and Snort IDS host |
| Kali Linux | 172.30.2.100 | Testing and traffic generation machine |
| Metasploitable2 | 172.30.2.101 | Vulnerable Linux target |
| Metasploitable3 | 172.30.2.102 | Vulnerable Windows target |

### Out of Scope

- Public IP addresses
- External networks
- Third-party systems
- Production environments
- Malware execution
- Unauthorized scanning
- Exploitation outside the isolated lab

---

## 3. Lab Environment

The lab was built as an isolated virtual network.

| Component | Purpose |
|---|---|
| pfSense | Gateway, firewall, and Snort IDS platform |
| Snort | Intrusion Detection System |
| Snort GPLv2 Community Rules | Baseline IDS ruleset |
| Kali Linux | Traffic generation and testing |
| Metasploitable2 | Vulnerable Linux target |
| Metasploitable3 | Vulnerable Windows target |
| Wazuh | SIEM platform available in the broader lab |

---

## 4. Tools Used

| Tool | Purpose |
|---|---|
| pfSense | Firewall and gateway |
| Snort | Intrusion detection |
| Snort GPLv2 Community Rules | Rule-based detection |
| Kali Linux | Testing and traffic generation |
| Nmap | Network validation and scan traffic |
| Netcat | TCP service connection testing |
| cURL | HTTP traffic generation |
| Custom Snort Rules | Detection logic for controlled lab traffic |

---

## 5. Methodology

The IDS workflow followed these phases:

1. Validate network connectivity.
2. Confirm pfSense gateway and LAN configuration.
3. Install and configure Snort on pfSense.
4. Enable Snort on the LAN interface.
5. Enable Snort GPLv2 Community Rules.
6. Create and enable custom Snort rules.
7. Generate controlled traffic from Kali Linux.
8. Validate Snort alerts.
9. Export alerts in CSV format.
10. Collect evidence and document the results.

---

## 6. Network Validation

The Kali machine was configured with IP address `172.30.2.100` and used pfSense as the default gateway at `172.30.2.1`.

Connectivity was validated using ICMP tests against the pfSense LAN interface and the Metasploitable2 target.

Commands used:

```bash
ping -c 4 172.30.2.1
ping -c 4 172.30.2.101
```

The first ping confirmed connectivity between Kali and the pfSense LAN interface.

The second ping confirmed connectivity between Kali and the Metasploitable2 target.

Both tests returned `0% packet loss`, confirming that the internal lab network was working correctly.

Evidence collected:

```text
evidence/01-network-validation.png
```

---

## 7. pfSense LAN Interface

The pfSense firewall was configured as the default gateway for the internal lab network.

| Interface | Role | IP Address |
|---|---|---|
| LAN | Internal network gateway | 172.30.2.1 |

This interface is responsible for routing traffic between the internal lab machines.

Evidence collected:

```text
evidence/02-pfsense-lan-interface.png
```

---

## 8. Snort Installation

Snort was installed on pfSense using the pfSense Package Manager.

After installation, the Snort service became available in the pfSense menu under:

```text
Services > Snort
```

The IDS was configured to monitor the LAN interface, where the internal lab hosts are connected.

Evidence collected:

```text
evidence/03-snort-installed.png
```

---

## 9. Snort Interface Configuration

Snort was enabled on the LAN interface to inspect internal lab traffic between the Kali testing machine and the vulnerable targets.

| Snort Interface | Status | Purpose |
|---|---|---|
| LAN | Enabled | Monitor internal lab traffic |

The LAN interface was selected because the traffic between Kali Linux and Metasploitable2 passes through the internal lab network.

After enabling the interface, Snort was started on LAN.

Evidence collected:

```text
evidence/04-snort-lan-interface-enabled.png
```

---

## 10. Snort Community Rules Configuration

The Snort GPLv2 Community Rules were enabled to provide a baseline ruleset for the IDS.

First, the community rules were enabled in:

```text
Services > Snort > Global Settings
```

The following option was selected:

```text
Enable Snort GPLv2 Community Rules
```

After saving the configuration, the rules were downloaded through:

```text
Services > Snort > Updates
```

Then, the ruleset was enabled on the LAN interface through:

```text
Services > Snort > Interface Settings > LAN Categories
```

The following ruleset was selected:

```text
Snort GPLv2 Community Rules
```

After saving the configuration, the active rules were validated under:

```text
Services > Snort > Interface Settings > LAN Rules
```

The `GPLv2_community.rules` category was available in the category selection dropdown, and several rules were already enabled by default.

This confirmed that the Snort GPLv2 Community Rules were successfully enabled, downloaded, and loaded on the LAN interface.

Evidence collected:

```text
evidence/05-snort-gplv2-community-rules-active.png
```

---

## 11. Custom Snort Rules Configuration

After enabling the Snort GPLv2 Community Rules, custom Snort rules were created to detect controlled lab traffic from Kali Linux to Metasploitable2.

The custom rules were added under the `custom.rules` category on the Snort LAN interface.

The objective of these rules was to validate that Snort could detect specific traffic generated inside the lab environment.

Custom rules used:

```snort
alert icmp 172.30.2.100 any -> 172.30.2.101 any (msg:"LAB ICMP Ping from Kali to Metasploitable2"; sid:1000001; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 22 (msg:"LAB SSH Connection Attempt from Kali to Metasploitable2"; sid:1000002; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 80 (msg:"LAB HTTP Connection Attempt from Kali to Metasploitable2"; sid:1000003; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 21 (msg:"LAB FTP Connection Attempt from Kali to Metasploitable2"; sid:1000004; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 any (msg:"LAB TCP Traffic from Kali to Metasploitable2"; sid:1000005; rev:1;)
```

The rules were created to detect ICMP traffic, SSH connection attempts, HTTP connection attempts, FTP connection attempts, and general TCP traffic from Kali Linux to the Metasploitable2 target.

After saving the custom rules, the Snort service was restarted on the LAN interface to apply the new configuration.

Evidence collected:

```text
evidence/06-snort-custom-rules.png
```

---

## 12. Traffic Generation from Kali Linux

Traffic was generated from the Kali Linux machine against the Metasploitable2 target in order to validate Snort detection.

The following commands were executed from Kali Linux:

```bash
ping -c 4 172.30.2.101

ssh 172.30.2.101

curl http://172.30.2.101

nc -vz 172.30.2.101 21

sudo nmap -sS 172.30.2.101

sudo nmap -A 172.30.2.101
```

The ICMP test was used to validate ping detection.

The SSH, HTTP, and FTP connection attempts were used to generate controlled service traffic.

The Nmap scans were used to generate reconnaissance traffic and validate additional Snort detections.

All tests were executed only inside the isolated local lab network.

Evidence collected:

```text
evidence/07-kali-ping-test.png
evidence/08-kali-service-tests.png
evidence/09-kali-nmap-scan.png
```

---

## 13. Snort Alert Validation

After generating traffic from Kali Linux, the Snort Alerts page was reviewed through:

```text
Services > Snort > Alerts
```

The alerts were filtered and reviewed on the LAN interface.

Snort generated alerts for the custom rules, confirming that the IDS was successfully inspecting internal lab traffic.

Expected custom alerts included:

```text
LAB ICMP Ping from Kali to Metasploitable2
LAB SSH Connection Attempt from Kali to Metasploitable2
LAB HTTP Connection Attempt from Kali to Metasploitable2
LAB FTP Connection Attempt from Kali to Metasploitable2
LAB TCP Traffic from Kali to Metasploitable2
```

Community rules could also generate additional alerts related to scan activity, reconnaissance, ICMP traffic, or TCP traffic.

| Source | Destination | Activity | Detection |
|---|---|---|---|
| 172.30.2.100 | 172.30.2.101 | ICMP Ping | Snort alert generated |
| 172.30.2.100 | 172.30.2.101 | SSH connection attempt | Snort alert generated |
| 172.30.2.100 | 172.30.2.101 | HTTP connection attempt | Snort alert generated |
| 172.30.2.100 | 172.30.2.101 | FTP connection attempt | Snort alert generated |
| 172.30.2.100 | 172.30.2.101 | Nmap SYN scan | Snort alert generated |
| 172.30.2.100 | 172.30.2.101 | Nmap aggressive scan | Snort alert generated |

This confirmed that Snort was correctly monitoring the LAN interface and generating alerts based on both custom rules and the enabled community ruleset.

The Snort alert export was also saved as a CSV file to preserve the detection results generated during the validation phase.

Evidence collected:

```text
evidence/10-snort-alerts-generated.png
evidence/snort-alerts-export.csv
```

---

## 14. Key Results

| Result | Description | Evidence |
|---|---|---|
| Network connectivity validated | Kali reached pfSense and Metasploitable2 successfully. | `evidence/01-network-validation.png` |
| pfSense LAN confirmed | pfSense LAN interface was configured as the internal gateway. | `evidence/02-pfsense-lan-interface.png` |
| Snort installed | Snort was installed through pfSense Package Manager. | `evidence/03-snort-installed.png` |
| Snort enabled on LAN | Snort monitored the internal LAN interface. | `evidence/04-snort-lan-interface-enabled.png` |
| Community rules enabled | Snort GPLv2 Community Rules were enabled and validated. | `evidence/05-snort-gplv2-community-rules-active.png` |
| Custom rules created | Custom rules were created for controlled lab detections. | `evidence/06-snort-custom-rules.png` |
| Traffic generated | Kali generated ICMP, service, and scan traffic. | `evidence/07-kali-ping-test.png`, `08-kali-service-tests.png`, `09-kali-nmap-scan.png` |
| Alerts generated | Snort generated alerts for the lab traffic. | `evidence/10-snort-alerts-generated.png` |
| Alerts exported | Alert results were exported to CSV. | `evidence/snort-alerts-export.csv` |

---

## 15. Detection Analysis

This project focused on detection rather than exploitation.

The alerts generated by Snort demonstrated the value of network-based intrusion detection for identifying controlled suspicious activity inside a lab network.

Key detection themes included:

- ICMP traffic detection
- Service connection attempt detection
- Reconnaissance-like traffic detection
- Nmap scan detection
- Custom rule validation
- Community rule validation
- Visibility into source and destination communication

In a real environment, these alerts would require triage, correlation with other logs, false positive review, and escalation only when supported by additional context.

---

## 16. Recommendations

General recommendations based on the project include:

- Keep IDS rules updated.
- Tune rules to reduce false positives.
- Document custom rules clearly.
- Monitor critical network segments.
- Export and review alerts regularly.
- Use segmentation to limit unnecessary traffic.
- Correlate IDS alerts with endpoint, firewall, and SIEM logs.
- Avoid overly broad rules in production environments.
- Maintain a clear evidence collection process.

---

## 17. Limitations

This assessment was performed in an isolated home lab environment.

The generated traffic and alerts were controlled and created for educational purposes.

The project does not represent a production SOC environment and does not include advanced log correlation, endpoint telemetry, or full incident response procedures.

No external systems, public IP addresses, or third-party networks were targeted.

---

## 18. Conclusion

The pfSense + Snort IDS deployment was successfully configured and validated.

Snort was enabled on the LAN interface, the GPLv2 Community Rules were loaded, and custom rules were created to detect controlled lab traffic.

After generating traffic from Kali Linux, Snort successfully generated alerts, confirming that the IDS was functioning correctly inside the Blue Team lab environment.

By documenting the process end to end, this project demonstrates practical Blue Team skills related to network monitoring, IDS operation, custom rule validation, alert review, and professional security documentation.
