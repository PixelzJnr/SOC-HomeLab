<<<<<<< HEAD
# SOC Home Lab — Real-Time Threat Detection with Wazuh

## Overview
A hands-on Security Operations Centre (SOC) home lab built to simulate real attacks and detect them in real time using Wazuh SIEM and Kali Linux.

## Lab Architecture

| Machine | OS | IP | Role |
|---|---|---|---|
| Wazuh Server | Amazon Linux | 192.168.56.101 | SIEM + Manager |
| Victim | Ubuntu 22.04 | 192.168.56.20 | Target endpoint |
| Attacker | Kali Linux 2026.1 | 192.168.56.30 | Attack simulation |

## Tools Used
- Wazuh 4.14.5 (SIEM, XDR, threat detection)
- Hydra (SSH brute force simulation)
- Nmap (network reconnaissance)
- VirtualBox 7.2 (virtualisation)
- MITRE ATT&CK framework (alert classification)

## Attacks Simulated
- Network reconnaissance with Nmap
- SSH brute force attack with Hydra (14 million passwords)

## Detections
- 538 medium severity alerts
- 1,657 low severity alerts
- MITRE ATT&CK tactics detected: Initial Access, Defense Evasion, Privilege Escalation, Persistence

## Screenshots
See the /screenshots folder for dashboard and attack evidence.

## Author
Frank Ewasy Odoom Jnr (Pixelz)
github.com/PixelzJnr
=======
# SOC-HomeLab
A hands-on SOC jome lab Wazuh SIEM, Kali Linux attacker and Ubuntu victim. This lab stimulates real attacks and detects them in real-time
>>>>>>> 249e7170163826e35b948b36b37f6e7a29430dd4
