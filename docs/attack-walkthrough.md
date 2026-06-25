# Attack Walkthrough — SOC Home Lab

**Author:** Frank Ewasy Odoom Jnr (Pixelz)  
**Date:** 15 June 2026  
**Environment:** VirtualBox · Wazuh 4.14.5 · Kali Linux 2026.1 · Ubuntu 22.04

---

## Objective

Simulate real-world attacks against a victim machine and validate that the Wazuh SIEM detects, classifies, and alerts on them in real time.

---

## Environment Summary

| Machine | IP | Role |
|---|---|---|
| Kali Linux | 192.168.56.30 | Attacker |
| Ubuntu 22.04 | 192.168.56.20 | Victim + Wazuh agent |
| Wazuh Server | 192.168.56.101 | SIEM + dashboard |

---

## Attack 1 — Network Reconnaissance with Nmap

### What I did
Ran a stealth SYN scan with service and OS detection against the victim machine from Kali:

```bash
sudo nmap -sS -sV 192.168.56.20
```

### What Nmap found
- Host is alive
- Port 22/tcp open — OpenSSH 8.9p1
- OS identified as Ubuntu Linux

### What Wazuh detected
- Network scan activity flagged
- MITRE ATT&CK tactic: Discovery (T1046 — Network Service Scanning)

### Why this matters
Reconnaissance is the first stage of most real attacks. Detecting it early gives defenders time to respond before exploitation begins.

---

## Attack 2 — SSH Brute Force with Hydra

### What I did
Ran a dictionary attack against the victim SSH service using the rockyou.txt wordlist:

```bash
hydra -l victim -P /usr/share/wordlists/rockyou.txt -t 4 ssh://192.168.56.20
```

### Attack statistics
- 14,344,399 passwords attempted
- ~70 attempts per minute
- 4 parallel tasks

### What Wazuh detected
- 538 medium severity alerts
- 1,657 low severity alerts
- MITRE ATT&CK tactics mapped:
  - T1110.001 — Password Guessing
  - Initial Access
  - Defense Evasion
  - Privilege Escalation
  - Persistence

### Why this matters
SSH brute force is one of the most common attack vectors against Linux servers. This test validated that Wazuh correctly identifies and escalates repeated authentication failures from the same source IP.

---

## Custom Detection Rules

Three custom Wazuh rules were written to improve detection accuracy:

| Rule ID | Trigger | Severity |
|---|---|---|
| 100001 | 5 failed SSH logins in 60 seconds | Level 10 — High |
| 100002 | 20 failed SSH logins in 120 seconds | Level 14 — Critical |
| 100003 | Successful login after brute force | Level 15 — Critical |

Rules are stored in `/rules/custom-ssh-bruteforce.xml`.

---

## Key Takeaways

- Wazuh successfully detected both passive reconnaissance and active brute force attacks
- MITRE ATT&CK mapping provides immediate context for alert triage
- Custom rules allow fine-tuning detection thresholds beyond default Wazuh behaviour
- Isolating the lab on a host-only network prevented any risk to the real network

---

## Next Steps

- Deploy Suricata for network-level intrusion detection
- Simulate privilege escalation and lateral movement
- Add a Windows victim machine to test AD-based attacks
- Integrate automated alerting via n8n and email notifications