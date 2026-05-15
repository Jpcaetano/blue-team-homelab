# Network Topology and IP Addressing

## Overview

This document describes the network topology and IP addressing plan used in my Blue Team cybersecurity home lab portfolio.

The lab is organized into two main network areas:

- **Internal Lab Network**: isolated virtual network used by lab virtual machines for security testing, scanning, monitoring, and controlled traffic generation.
- **Host / Management Network**: network used by the physical Windows 11 host to access management dashboards and cloud-based consoles.

All security testing activities are performed in a controlled lab environment. No public IP addresses, third-party networks, or production systems are included in the lab target scope.

---

## High-Level Network Diagram

```text
                    Host / Management Network
                         192.168.6.0/24
                                |
                    Physical Windows 11 Host
                   192.168.6.x / Host-side IP
                                |
        -------------------------------------------------
        |                                               |
 Action1 Cloud Console                         Wazuh Dashboard Access
 Endpoint Management                            Browser access from host
        |
 Action1 Agent on Windows 11
 Outside pfSense LAN


                    Internal Lab Network
                         172.30.2.0/24
                                |
                             pfSense
                            172.30.2.1
                                |
        -------------------------------------------------
        |                       |                       |
   Kali Linux             Metasploitable2         Metasploitable3
  172.30.2.100             172.30.2.101            172.30.2.102
        |
 Nessus / Nmap
 Scanner Machine

                                |
                           Wazuh Server
                           172.30.2.104
                    Internal lab communication
```

---

## Network Segmentation

### Internal Lab Network

| Field | Value |
|---|---|
| Network | `172.30.2.0/24` |
| Gateway | `172.30.2.1` |
| Firewall | `pfSense` |
| Purpose | Internal communication between lab virtual machines |

The internal lab network is the main isolated environment used for vulnerability scanning, IDS validation, SIEM testing, and controlled Blue Team exercises.

### Host / Management Network

| Field | Value |
|---|---|
| Network | `192.168.6.0/24` |
| Host System | Physical Windows 11 machine |
| Purpose | Dashboard access, browser-based management, and cloud console access |

The physical Windows 11 host is outside the pfSense LAN. It is not part of the internal `172.30.2.0/24` lab network.

This host is used to:

- Access the Wazuh dashboard through the management/host-side interface.
- Access the Action1 cloud console.
- Run the Action1 agent as a real monitored endpoint for endpoint management testing.
- Manage the virtual lab environment through VirtualBox.

---

## Addressing Strategy

The internal lab uses a simple static addressing model to make scanning, evidence collection, firewall configuration, and documentation easier to reproduce.

| Range / Address | Purpose |
|---|---|
| `172.30.2.1` | pfSense gateway and firewall |
| `172.30.2.100` | Main security testing and scanning machine |
| `172.30.2.101-102` | Intentionally vulnerable lab targets |
| `172.30.2.104` | SIEM server |
| `172.30.2.105+` | Reserved for future internal lab endpoints |

This structure keeps the lab predictable and makes it easier to reference systems across project documentation, screenshots, reports, and security tool configurations.

---

## Internal Lab Assets

| Host | Role | IP Address | Notes |
|---|---|---:|---|
| pfSense | Gateway / Firewall / IDS | `172.30.2.1` | LAN interface and default gateway for the internal lab |
| Kali Linux | Scanner / Testing Machine | `172.30.2.100` | Nessus, Nmap, traffic generation, and security testing tools |
| Metasploitable2 | Vulnerable Linux Target | `172.30.2.101` | Vulnerability scanning and IDS testing target |
| Metasploitable3 | Vulnerable Windows Target | `172.30.2.102` | Vulnerability scanning and Windows-based testing target |
| Wazuh Server | SIEM / Log Analysis | `172.30.2.104` | Internal lab interface used for agent communication and log collection |

> Note: The physical Windows 11 host is intentionally not listed as an internal lab asset because it is outside the pfSense LAN.

---

## Host / Management Assets

| Host | Role | IP Address | Notes |
|---|---|---:|---|
| Windows 11 Host | Physical Host / Endpoint | `192.168.6.x` / Host-side IP | Used to manage the lab, access dashboards, and run the Action1 agent |
| Wazuh Server | SIEM Dashboard Access | `192.168.6.x` / Host-side IP | Bridge or host-access interface used to access the Wazuh dashboard from the Windows host |
| Action1 Cloud Console | Endpoint Management Console | Cloud-based | Used to manage and monitor the Windows 11 endpoint through the Action1 agent |

---

## Network Roles

### pfSense

pfSense acts as the internal lab gateway, firewall, and IDS platform.

| Interface | Role | IP Address |
|---|---|---:|
| LAN | Internal lab gateway | `172.30.2.1` |

The LAN interface is used as the default gateway by the internal lab machines.

Main use cases:

- Internal lab routing
- Firewall rule testing
- Snort IDS deployment
- Controlled network traffic monitoring

---

### Kali Linux

Kali Linux is used as the main security testing and scanning machine.

Assigned IP:

```text
172.30.2.100
```

Common use cases:

- Nmap scanning
- Nessus vulnerability scanning
- Traffic generation
- IDS validation
- Lab connectivity testing

---

### Metasploitable2

Metasploitable2 is used as an intentionally vulnerable Linux target.

Assigned IP:

```text
172.30.2.101
```

Common use cases:

- Vulnerability scanning
- IDS alert testing
- Service enumeration
- Controlled lab traffic generation

---

### Metasploitable3

Metasploitable3 is used as an intentionally vulnerable Windows target.

Assigned IP:

```text
172.30.2.102
```

Common use cases:

- Vulnerability scanning
- Windows service enumeration
- RDP and SMB-related testing
- Web and application vulnerability discovery

---

### Wazuh Server

The Wazuh server is used for SIEM and log analysis projects.

Internal lab IP:

```text
172.30.2.104
```

The Wazuh server may also have a host-side or bridged interface used only for dashboard access from the physical Windows 11 host.

| Interface Purpose | IP Address | Notes |
|---|---:|---|
| Internal lab communication | `172.30.2.104` | Used for communication with agents and lab endpoints |
| Dashboard / host access | `192.168.6.x` / Host-side IP | Used by the Windows host to access the Wazuh dashboard |

> Note: Wazuh may be out of scope for projects focused only on firewall, IDS, vulnerability scanning, or endpoint management. For example, it was intentionally excluded from Project 02 - Vulnerability Management.

---

### Windows 11 Host

The Windows 11 system is the physical host machine and is outside the pfSense LAN.

It is not part of the internal `172.30.2.0/24` network.

Host-side IP:

```text
192.168.6.x
```

Common use cases:

- VirtualBox host machine
- Browser access to Wazuh dashboard
- Access to cloud-based security consoles
- Action1 agent deployment
- Endpoint management testing
- Evidence collection and documentation

For Project 04, this Windows 11 host is used as the real endpoint managed through Action1.

---

### Action1 Cloud Console

Action1 is used as the endpoint management platform for Project 04.

The Action1 console is cloud-based and does not reside inside the internal pfSense LAN.

Common use cases:

- Endpoint visibility
- Agent deployment validation
- Patch management overview
- Software inventory review
- Remote endpoint management features

---

## Access Notes

- The pfSense LAN interface is the default gateway for the internal lab network.
- Internal lab virtual machines communicate through the `172.30.2.0/24` network.
- The physical Windows 11 host is outside the pfSense LAN.
- The Windows 11 host uses the `192.168.6.0/24` host/management network.
- The Wazuh dashboard can be accessed from the Windows host through the Wazuh server host-side interface.
- The Action1 console is accessed through a web browser from the Windows 11 host.
- The Action1 agent installed on Windows 11 communicates with the Action1 cloud platform, not through the internal pfSense LAN.

---

## Project Usage

This topology and addressing plan support the following home lab projects:

| Project | Main Systems Used |
|---|---|
| Project 01 - pfSense + Snort IDS | pfSense, Kali Linux, Metasploitable2 |
| Project 02 - Vulnerability Management | Kali Linux, Nessus, Metasploitable2, Metasploitable3 |
| Project 03 - Wazuh SIEM | Wazuh Server, Windows 11 Host, lab endpoints |
| Project 04 - Endpoint Management with Action1 | Windows 11 Host, Action1 Agent, Action1 Cloud Console |

---

## Security Notes

- This IP addressing plan uses private RFC1918 address ranges.
- The lab is intended for local educational and portfolio use only.
- No public IP addresses are part of the lab target scope.
- Vulnerable machines must remain isolated from production environments.
- Security testing should only be performed against systems owned and controlled inside the lab.
- The Windows 11 host is used carefully as a management and endpoint testing system, not as a vulnerable target inside the pfSense LAN.
- Cloud-based tools such as Action1 are documented as management platforms, not internal lab assets.

---

## Status

Active.
