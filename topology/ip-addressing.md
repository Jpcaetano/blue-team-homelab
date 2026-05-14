\# IP Addressing



This document describes the IP addressing plan used in the cybersecurity homelab.



\## Internal Lab Network



Network: `172.30.2.0/24`  

Gateway: `172.30.2.1`  

Firewall: `pfSense`  



| Host | Role | IP Address | Notes |

|---|---|---|---|

| pfSense | Gateway / Firewall / IDS | 172.30.2.1 | LAN interface and default gateway |

| Kali Linux | Scanner / Testing machine | 172.30.2.100 | Nessus, Nmap and security testing tools |

| Metasploitable 2 | Vulnerable Linux target | 172.30.2.101 | Vulnerability scanning target |

| Metasploitable 3 | Vulnerable Windows target | 172.30.2.102 | Vulnerability scanning target |

| Wazuh Server | SIEM / Log analysis | 172.30.2.104 | Internal lab interface for Wazuh agents |

| Windows Endpoint | Monitored endpoint | 172.30.2.105 | Future Windows agent VM |



\## Management / Host Access Network



Network: `192.168.6.0/24`



| Host | Role | IP Address | Notes |

|---|---|---|---|

| Windows Host | Physical host machine | 192.168.6.x | Used to access dashboards and manage the lab |

| Wazuh Server | SIEM Dashboard | 192.168.6.x | Bridge interface used to access the Wazuh dashboard from Windows |



\## Access Notes



\- The pfSense LAN interface is the default gateway for the internal lab network.

\- Internal lab machines communicate through the `172.30.2.0/24` network.

\- The Wazuh Server has two network interfaces:

&#x20; - `172.30.2.104` for communication with lab endpoints and agents.

&#x20; - `192.168.6.x` for dashboard access from the Windows host.

\- The Wazuh Dashboard can be accessed from the Windows host using:

