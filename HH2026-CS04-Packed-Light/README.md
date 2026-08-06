# 🛰️ Packed Light

![Platform](https://img.shields.io/badge/Platform-TryHackMe-red?style=for-the-badge)
![Event](https://img.shields.io/badge/Event-Hacker%20Holidays%202026-6f42c1?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Network%20Forensics-0f8b8d?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-success?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Tool](https://img.shields.io/badge/Primary%20Tool-Wireshark-blue?style=for-the-badge)

---

> **Hacker Holidays 2026 – Day 4**  
> **Category:** Network Forensics  
> **Difficulty:** Easy

---

# 📌 Overview

**Packed Light** is a Network Forensics challenge from **Hacker Holidays 2026** focused on discovering a covert communication channel hidden inside HTTP traffic.

The challenge simulates malware secretly exfiltrating encrypted keystrokes by embedding them inside HTTP Cookie headers. Through packet capture analysis, malware source code review, and cryptographic analysis, the hidden communication was successfully reconstructed.

---

# 🎯 Objectives

- Analyze the provided PCAP file.
- Identify the covert communication channel.
- Recover hidden exfiltrated data.
- Reverse the custom encryption routine.
- Decode the recovered payload.
- Retrieve the challenge flag.

---

# 🔍 Key Finding

The malware transmitted encrypted keystrokes inside the HTTP Cookie:

```http
hotel_sess_state
```

Every key press followed the workflow:

```
Keystroke
     ↓
XOR Encryption
     ↓
Base64 Encoding
     ↓
HTTP Cookie
     ↓
HTTP GET Request
```

Recovered XOR Key:

```text
H0t3lSt@ff0NlyK3epS3cr3t!
```

Recovered Flag:

```text
THM{V3r4_1s_w4tch1ng_0veR_y0u}
```

---

# ⚙️ Methodology

## 1. Packet Analysis

- Opened the supplied PCAP file in Wireshark.
- Filtered traffic:

```wireshark
tcp.port == 8080
```

---

## 2. HTTP Traffic Inspection

Identified repetitive HTTP GET requests containing cookies such as:

```http
Cookie: hotel_sess_state=HA==
Cookie: hotel_sess_state=AA==
Cookie: hotel_sess_state=Mw==
...
```

---

## 3. HTTP Object Extraction

Exported HTTP Objects.

Recovered:

```python
updates.py
```

---

## 4. Source Code Review

Recovered malware logic:

- XOR Encryption
- Base64 Encoding
- HTTP Cookie Exfiltration

Recovered secret key:

```text
H0t3lSt@ff0NlyK3epS3cr3t!
```

---

## 5. Payload Recovery

```
HTTP Cookie
      ↓
Base64 Decode
      ↓
XOR Decryption
      ↓
Recovered Plaintext
```

Successfully reconstructed the hidden communication and extracted the flag.

---

# 🧰 Tools Used

- Wireshark
- Python
- CyberChef
- Linux CLI
- Base64
- XOR Analysis

---

# 🧠 Skills Practiced

- Network Forensics
- Packet Capture (PCAP) Analysis
- Wireshark Packet Filtering
- HTTP Traffic Analysis
- HTTP Stream Inspection
- HTTP Object Extraction
- Malware Traffic Analysis
- Python Code Review
- Base64 Decoding
- XOR Cryptanalysis
- Covert Channel Detection
- Incident Investigation

---

# 📸 Evidence

## 🖼️ Challenge Overview

```
images/challenge-overview.png
```

---

## 🖼️ Wireshark Analysis

```
images/wireshark-analysis.png
```

---

## 🖼️ Recovered Flag

```
images/recovered-flag.png
```

---

# 📄 Case Study

A professional one-page cybersecurity case study is included in this repository.

```
HH2026_Day4_Packed_Light_Case_Study.pdf
```

---

# 🎓 Lessons Learned

- HTTP Cookies can be abused as covert communication channels.
- Malware often hides encrypted data inside legitimate application traffic.
- Base64 encoding alone is not encryption; it is frequently combined with XOR or other ciphers.
- Reviewing recovered source code significantly accelerates malware analysis.
- Network forensics combines protocol analysis with reverse engineering to uncover attacker techniques.

---

# 🏷️ Tags

`#TryHackMe`
`#HackerHolidays2026`
`#NetworkForensics`
`#PCAP`
`#Wireshark`
`#HTTP`
`#MalwareAnalysis`
`#CyberSecurity`
`#Base64`
`#XOR`
`#Python`

---

# 👩‍💻 Author

**Samyuktha Saravanan**

🔗 GitHub: https://github.com/samyuktha200512-ops

---

⭐ If you found this repository useful, consider giving it a **Star**!
