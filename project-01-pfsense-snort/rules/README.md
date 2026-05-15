# Custom Snort Rules

## Overview

This directory contains custom Snort rules created for **Project 01 - pfSense + Snort IDS**.

The rules were designed to detect controlled traffic generated from Kali Linux (`172.30.2.100`) against the Metasploitable2 target (`172.30.2.101`).

The purpose of this file is to document the rules, their intent, and their expected behavior during the lab validation.

---

## Scope

These rules were created and tested only inside an isolated Blue Team home lab environment.

### In Scope

- Kali Linux: `172.30.2.100`
- Metasploitable2: `172.30.2.101`
- pfSense LAN interface
- Snort IDS running on pfSense
- Controlled lab traffic

### Out of Scope

- Public IP addresses
- External networks
- Third-party systems
- Production environments
- Unauthorized scanning or testing

---

## Rule File

The custom rules are stored in:

```text
custom-snort-rules.rules
```

---

## Custom Rules

```snort
alert icmp 172.30.2.100 any -> 172.30.2.101 any (msg:"LAB ICMP Ping from Kali to Metasploitable2"; sid:1000001; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 22 (msg:"LAB SSH Connection Attempt from Kali to Metasploitable2"; sid:1000002; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 80 (msg:"LAB HTTP Connection Attempt from Kali to Metasploitable2"; sid:1000003; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 21 (msg:"LAB FTP Connection Attempt from Kali to Metasploitable2"; sid:1000004; rev:1;)

alert tcp 172.30.2.100 any -> 172.30.2.101 any (msg:"LAB TCP Traffic from Kali to Metasploitable2"; sid:1000005; rev:1;)
```

---

## Rule Summary

| SID | Protocol | Source | Destination | Port | Description |
|---|---|---|---|---|---|
| 1000001 | ICMP | 172.30.2.100 | 172.30.2.101 | any | Detects ICMP ping traffic from Kali to Metasploitable2 |
| 1000002 | TCP | 172.30.2.100 | 172.30.2.101 | 22 | Detects SSH connection attempts |
| 1000003 | TCP | 172.30.2.100 | 172.30.2.101 | 80 | Detects HTTP connection attempts |
| 1000004 | TCP | 172.30.2.100 | 172.30.2.101 | 21 | Detects FTP connection attempts |
| 1000005 | TCP | 172.30.2.100 | 172.30.2.101 | any | Detects general TCP traffic from Kali to Metasploitable2 |

---

## Testing Methodology

The rules were tested by generating controlled traffic from Kali Linux to Metasploitable2.

Example commands used during testing:

```bash
ping -c 4 172.30.2.101

ssh 172.30.2.101

curl http://172.30.2.101

nc -vz 172.30.2.101 21

sudo nmap -sS 172.30.2.101

sudo nmap -A 172.30.2.101
```

After generating traffic, Snort alerts were reviewed in:

```text
Services > Snort > Alerts
```

---

## Notes

These rules were created for a controlled lab environment and were used to validate Snort alert generation on the pfSense LAN interface.

The rule with SID `1000005` is intentionally broad and was used only for validation purposes. In a production environment, overly broad rules can generate a large number of alerts and should be refined to reduce noise.

Recommended production considerations:

- Avoid overly broad source and destination definitions.
- Use specific ports and services when possible.
- Add metadata where appropriate.
- Tune rules based on expected network behavior.
- Review false positives before enabling blocking actions.
- Maintain clear documentation for each custom rule.

---

## Disclaimer

These custom rules are intended for educational and portfolio purposes only.

They are not production-ready rules and should not be used in real environments without proper testing, tuning, and validation.
