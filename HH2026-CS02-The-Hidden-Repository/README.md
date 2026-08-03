# 🌐 HH2026 – Room 404

![Web Security](https://img.shields.io/badge/Web%20Security-blue?style=for-the-badge)
![Directory Enumeration](https://img.shields.io/badge/Directory%20Enumeration-orange?style=for-the-badge)
![Git Exposure](https://img.shields.io/badge/Git%20Exposure-red?style=for-the-badge)
![Source Code Review](https://img.shields.io/badge/Source%20Code%20Review-darkgreen?style=for-the-badge)
![TryHackMe](https://img.shields.io/badge/TryHackMe-HH2026-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## Overview

This challenge focused on identifying sensitive information exposed through an improperly configured web application. The objective was to discover hidden resources, recover the exposed source code, and identify confidential information left within the application's repository.

---

## Challenge Objective

- Enumerate the web application.
- Recover the exposed source code.
- Locate the hidden flag.

---

## Skills Practiced

- Web Enumeration
- Directory Enumeration
- Source Code Review
- Git Exposure Analysis
- Information Disclosure

---

## Methodology

### 1. Initial Reconnaissance

The web application was accessed through the provided target.

A standard directory enumeration was performed using **Gobuster**.

### 2. Hidden Resource Discovery

Directory enumeration identified an exposed Git repository.

```
/.git/HEAD
```

This indicated that the developer had unintentionally exposed the application's version control data.

### 3. Source Code Recovery

The exposed Git repository was downloaded using **git-dumper**, allowing recovery of the complete application source code.

Recovered files included:

- README.md
- app.js
- index.html

### 4. Source Code Review

The recovered files were manually reviewed.

The confidential information required to complete the challenge was located within the recovered repository.

---

## Technical Findings

### Vulnerability

**Exposed Git Repository (.git Disclosure)**

The web server allowed public access to the application's Git metadata.

This allowed an attacker to reconstruct the entire project source code without authentication.

### Risk

An exposed Git repository may disclose:

- Application source code
- Internal documentation
- Configuration files
- API endpoints
- Credentials
- Secrets
- Hardcoded tokens
- Business logic

---

## Lessons Learned

- Never expose the `.git` directory on production servers.
- Version control repositories frequently contain sensitive information.
- Directory enumeration remains one of the most effective techniques during web reconnaissance.
- Always review recovered source code carefully before searching for vulnerabilities.

---

## Tools Used

- Gobuster
- git-dumper
- Linux Terminal

---

## References

- TryHackMe – Hacker Holidays 2026
- OWASP – Information Disclosure
