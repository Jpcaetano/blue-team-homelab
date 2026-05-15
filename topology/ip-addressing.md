# IP Addressing

## Overview

This document describes the IP addressing plan used in the Blue Team cybersecurity home lab.

The lab is separated into two main network areas:

- **Internal Lab Network**: used by the virtual machines for security testing, scanning, monitoring, and lab traffic.
- **Management / Host Access Network**: used by the physical Windows host to access dashboards and manage the lab.

All lab activity is performed in an isolated virtual environment. No public IP addresses, external systems, third-party networks, or production environments are included in the lab scope.

---

## Internal Lab Network

| Field | Value |
|---|---|
| Network | `172.30.2.0/24` |
| Gateway | `172.30.2.1` |
| Firewall | `pfSense` |
| Purpose | Internal lab communication between virtual machines |

### Internal Assets

| Host | Role | IP Address | Notes |
|---|---|---:|---|
| pfSense | Gateway / Firewall / IDS | 172.30.2.1 | LAN interface and default gateway for the lab |
| Kali Linux | Scanner / Testing Machine | 172.30.2.100 | Nessus, Nmap, traffic generation, and security testing tools |
| Metasploitable2 | Vulnerable Linux Target | 172.30.2.101 | Vulnerability scanning and IDS testing target |
| Metasploitable3 | Vulnerable Windows Target | 172.30.2.102 | Vulnerability scanning and Windows-based testing target |
| Wazuh Server | SIEM / Log Analysis | 172.30.2.104 | Internal lab interface for Wazuh agents |
| Windows Endpoint | Monitored Endpoint | 172.30.2.105 | Future Windows agent VM for endpoint monitoring |

---

## Management / Host Access Network

| Field | Value |
|---|---|
| Network | `192.168.6.0/24` |
| Purpose | Host-side access to lab dashboards and management interfaces |

### Management Assets

| Host | Role | IP Address | Notes |
|---|---|---:|---|
| Windows Host | Physical Host Machine | 192.168.6.x | Used to access dashboards and manage the lab |
| Wazuh Server | SIEM Dashboard | 192.168.6.x | Bridge interface used to access the Wazuh dashboard from the Windows host |

---

## Network Roles

### pfSense

pfSense acts as the gateway and firewall for the internal lab network.

| Interface | Role | IP Address |
|---|---|---:|
| LAN | Internal lab gateway | 172.30.2.1 |

The LAN interface is used as the default gateway by the internal lab machines.

---

### Kali Linux

Kali Linux is used as the main security testing and scanning machine.

Common use cases:

- Nmap scanning
- Nessus vulnerability scanning
- Traffic generation
- IDS validation
- Lab connectivity testing

Assigned IP:

```text
172.30.2.100
```

---

### Metasploitable2

Metasploitable2 is used as an intentionally vulnerable Linux target.

Common use cases:

- Vulnerability scanning
- IDS alert testing
- Service enumeration
- Controlled lab traffic generation

Assigned IP:

```text
172.30.2.101
```

---

### Metasploitable3

Metasploitable3 is used as an intentionally vulnerable Windows target.

Common use cases:

- Vulnerability scanning
- Windows service enumeration
- RDP and SMB-related testing
- Web and application vulnerability discovery

Assigned IP:

```text
172.30.2.102
```

---

### Wazuh Server

The Wazuh server is used for SIEM and log analysis projects.

It has two network interfaces:

| Interface Purpose | IP Address | Notes |
|---|---:|---|
| Internal lab communication | 172.30.2.104 | Used for communication with agents and lab endpoints |
| Dashboard / host access | 192.168.6.x | Used by the Windows host to access the Wazuh dashboard |

> Note: Wazuh may be out of scope for projects focused only on firewall, IDS, or vulnerability scanning. For example, it was intentionally excluded from Project 02 - Vulnerability Management.

---

### Windows Endpoint

The Windows Endpoint is reserved for future endpoint monitoring and agent-based telemetry.

Common planned use cases:

- Wazuh agent deployment
- Endpoint log collection
- Security event monitoring
- Blue Team detection scenarios

Assigned IP:

```text
172.30.2.105
```

---

## Access Notes

- The pfSense LAN interface is the default gateway for the internal lab network.
- Internal lab machines communicate through the `172.30.2.0/24` network.
- The Windows host uses the `192.168.6.0/24` management network to access dashboards and management interfaces.
- The Wazuh Server has two network interfaces:
  - `172.30.2.104` for communication with lab endpoints and agents.
  - `192.168.6.x` for dashboard access from the Windows host.
- The Wazuh Dashboard can be accessed from the Windows host using the Wazuh server management IP.

---

## Project Usage

This addressing plan supports the following home lab projects:

| Project | Main Systems Used |
|---|---|
| Project 01 - pfSense + Snort IDS | pfSense, Kali Linux, Metasploitable2 |
| Project 02 - Vulnerability Management | Kali Linux, Nessus, Metasploitable2, Metasploitable3 |
| Project 03 - Wazuh SIEM | Wazuh Server, Windows Endpoint, Kali Linux |
| Project 04 - Endpoint Management | Windows Endpoint, Wazuh Agent, Wazuh Server |

---

## Security Notes

- This IP addressing plan uses private RFC1918 address ranges.
- The lab is intended for local educational use only.
- No public IP addresses are part of the lab target scope.
- Vulnerable machines must remain isolated from production networks.
- Security testing should only be performed against systems owned and controlled inside the lab.

---

## Status

Active.
