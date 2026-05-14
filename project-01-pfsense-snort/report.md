\# Project 01 - pfSense + Snort IDS



\## 1. Network Validation



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



`01-network-validation.png`



\---



\## 2. pfSense LAN Interface



The pfSense firewall was configured as the default gateway for the internal lab network.



| Interface | Role | IP Address |

|---|---|---|

| LAN | Internal network gateway | 172.30.2.1 |



This interface is responsible for routing traffic between the internal lab machines.



Evidence collected:



`02-pfsense-lan-interface.png`



\---



\## 3. Snort Installation



Snort was installed on pfSense using the pfSense Package Manager.



After installation, the Snort service became available in the pfSense menu under:



```text

Services > Snort

```



The IDS was configured to monitor the LAN interface, where the internal lab hosts are connected.



Evidence collected:



`03-snort-installed.png`



\---



\## 4. Snort Interface Configuration



Snort was enabled on the LAN interface to inspect internal lab traffic between the Kali testing machine and the vulnerable targets.



| Snort Interface | Status | Purpose |

|---|---|---|

| LAN | Enabled | Monitor internal lab traffic |



The LAN interface was selected because the traffic between Kali Linux and Metasploitable2 passes through the internal lab network.



After enabling the interface, Snort was started on LAN.



Evidence collected:



`04-snort-lan-interface-enabled.png`



\---



\## 5. Snort Community Rules Configuration



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



The `GPLv2\_community.rules` category was available in the category selection dropdown, and several rules were already enabled by default.



This confirmed that the Snort GPLv2 Community Rules were successfully enabled, downloaded, and loaded on the LAN interface.



Evidence collected:



`05-snort-gplv2-community-rules-active.png`



\---



\## 6. Custom Snort Rules Configuration



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



`06-snort-custom-rules.png`



\---



\## 7. Traffic Generation from Kali Linux



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



`07-kali-ping-test.png`



`08-kali-service-tests.png`



`09-kali-nmap-scan.png`



\---



\## 8. Snort Alert Validation



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



`10-snort-alerts-generated.png`



`snort-alerts-export.csv`





