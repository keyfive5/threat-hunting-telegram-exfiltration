# Threat Hunting: Detecting Data Exfiltration & Post-Exploitation Activity

## 📌 Overview
This repository showcases multiple **threat hunting investigations** across network traffic (PCAP) and endpoint logs to identify real-world attacker behavior.

The project demonstrates detection of:
- Data exfiltration via Telegram Bot API  
- Credential-stealing malware (Racoon Stealer)  
- Malicious PowerShell post-exploitation activity  
- LOLBAS-based defense evasion using trusted Windows binaries  

---

## 🎯 Objective
- Identify compromised hosts  
- Detect data exfiltration techniques  
- Analyze attacker behavior (TTPs)  
- Develop detection and response strategies  

---

# 🔍 Investigation 1: Telegram Data Exfiltration (PCAP Analysis)

## Key Findings
- Compromised host communicating with Telegram API  
- Destination: `149.154.167.220 (api.telegram.org)`  
- Protocol: HTTPS (TLS encrypted)  
- Exfiltration via Telegram Bot API (`/sendMessage`)  
- JSON-based structured data transfer  

## Analysis Highlights
- Filter used: ip.addr == 149.154.167.0/24
- - TLS sessions decrypted using SSL key logs  
- Identified HTTP POST requests sending data externally  

## Threat Insight
Attackers leveraged:
- Encrypted traffic (TLS)  
- Legitimate platform (Telegram)  

➡️ This is a common **covert C2 + exfiltration technique**

---

# 🔍 Investigation 2: Malicious PowerShell Activity (Log Analysis)

## Key Findings
- Obfuscated PowerShell execution: powershell.exe -nop -w hidden -EncodedCommand
- Abuse of Windows Defender exclusion: Add-MpPreference


➡️ Used to bypass detection and maintain persistence :contentReference[oaicite:0]{index=0}  

## Critical Indicators
- Hidden execution (`-w hidden`)  
- No profile (`-nop`)  
- Encoded payloads  

## Threat Insight
- Fileless malware behavior  
- Defense evasion  
- Post-exploitation activity  
- Execution under **Administrator/SYSTEM privileges**  

---

# 🔍 Investigation 3: Racoon Stealer Malware (PCAP Analysis)

## Key Findings
- Credential-stealing malware activity  
- Suspicious HTTP POST traffic  
- Data exfiltration via `/gate.php` endpoints  
- Plaintext credential data observed in packets  

➡️ Indicates **active data theft and exfiltration** :contentReference[oaicite:1]{index=1}  

## Threat Insight
- Use of fake headers to mimic legitimate traffic  
- Chunked data uploads  
- Known malicious C2 infrastructure  

---

# 🛡️ Detection Engineering & Playbook

## LOLBAS Evasion Detection

### Technique:
- Abuse of `wuauclt.exe` (trusted Windows binary)  
- Malicious DLL sideloading  

### Indicators:
- Unsigned DLL (`helpa.dll`) loaded  
- Suspicious command: wuauclt.exe /UpdateDeploymentProvider
- Child process:cmd.exe


➡️ Classic **defense evasion using trusted binaries** :contentReference[oaicite:2]{index=2}  

---

## Detection Strategy
- Sigma rules for:
- PowerShell obfuscation  
- Defender exclusion abuse  
- Hidden file manipulation  

---

# 🛠️ Tools & Technologies
- Wireshark  
- PCAP Analysis  
- TLS Decryption  
- Log Analysis  
- Sigma Rules  
- Suricata  
- Flowsynth  

---

# 📁 Repository Structure
.
├── data/
│ ├── 21.pcap
│ └── Racoon.pcap
├── logs/
│ └── suspicious_activity.log
├── rules/
│ ├── rule1_add-mppreference.yaml
│ └── rule2_attrib-hidden-files.yaml
├── simulation/
│ └── cve2025.fs
├── report/
│ ├── telegram_case_study.pdf
│ ├── powershell_investigation.pdf
│ └── racoon_analysis.pdf
├── playbook/
│ └── lolbas_detection_playbook.pdf
└── README.md

---

# 🔐 Detection Opportunities

- Monitor outbound traffic to Telegram API  
- Detect abnormal HTTPS POST patterns  
- Alert on encoded PowerShell execution  
- Flag `Add-MpPreference` usage  
- Detect unsigned DLL execution via trusted binaries  
- Monitor suspicious HTTP exfiltration patterns  

---

# 🧾 Conclusion

This repository demonstrates:
- **Network threat hunting** (PCAP analysis)  
- **Endpoint threat hunting** (log analysis)  
- **Malware detection** (Racoon Stealer)  
- **Detection engineering** (Sigma + Suricata)  

These findings represent **high-confidence compromise scenarios**, including:
- Data exfiltration  
- Credential theft  
- Defense evasion  
- Privileged attacker activity  

---

# ⚠️ Dataset Notice
All data used in this repository is for **educational and research purposes only**.  
No sensitive or personal information is intentionally included.

---

# 🚀 Key Takeaway
Effective threat hunting is about:
> **thinking like an attacker and proving compromise through behavior.**

---

# 👤 Author
Muhammad Hasan Zafar  
Cybersecurity Graduate | Threat Hunting | Detection Engineering
