\# Custom Snort Rules



This file contains custom Snort rules created for the \*\*Project 01 - pfSense + Snort IDS\*\* lab.



The rules were designed to detect controlled traffic generated from Kali Linux (`172.30.2.100`) against the Metasploitable2 target (`172.30.2.101`).



\## Custom Rules



```snort

alert icmp 172.30.2.100 any -> 172.30.2.101 any (msg:"LAB ICMP Ping from Kali to Metasploitable2"; sid:1000001; rev:1;)



alert tcp 172.30.2.100 any -> 172.30.2.101 22 (msg:"LAB SSH Connection Attempt from Kali to Metasploitable2"; sid:1000002; rev:1;)



alert tcp 172.30.2.100 any -> 172.30.2.101 80 (msg:"LAB HTTP Connection Attempt from Kali to Metasploitable2"; sid:1000003; rev:1;)



alert tcp 172.30.2.100 any -> 172.30.2.101 21 (msg:"LAB FTP Connection Attempt from Kali to Metasploitable2"; sid:1000004; rev:1;)



alert tcp 172.30.2.100 any -> 172.30.2.101 any (msg:"LAB TCP Traffic from Kali to Metasploitable2"; sid:1000005; rev:1;)

```



\## Rule Summary



| SID | Protocol | Source | Destination | Port | Description |

|---|---|---|---|---|---|

| 1000001 | ICMP | 172.30.2.100 | 172.30.2.101 | any | Detects ICMP ping traffic from Kali to Metasploitable2 |

| 1000002 | TCP | 172.30.2.100 | 172.30.2.101 | 22 | Detects SSH connection attempts |

| 1000003 | TCP | 172.30.2.100 | 172.30.2.101 | 80 | Detects HTTP connection attempts |

| 1000004 | TCP | 172.30.2.100 | 172.30.2.101 | 21 | Detects FTP connection attempts |

| 1000005 | TCP | 172.30.2.100 | 172.30.2.101 | any | Detects general TCP traffic from Kali to Metasploitable2 |



\## Notes



These rules were created for a controlled lab environment and were used to validate Snort alert generation on the pfSense LAN interface.



The rule with SID `1000005` is intentionally broad and was used only for validation purposes. In a production environment, overly broad rules can generate a large number of alerts and should be refined to reduce noise.

