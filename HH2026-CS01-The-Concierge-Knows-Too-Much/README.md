# 🛡️ HH2026 Case Study #01

![AI Security](https://img.shields.io/badge/AI-Security-purple)
![Prompt Injection](https://img.shields.io/badge/Prompt-Injection-red)
![LLM Security](https://img.shields.io/badge/LLM-Security-blue)
![Social Engineering](https://img.shields.io/badge/Social-Engineering-orange)
![Authorization](https://img.shields.io/badge/Authorization-Analysis-darkgreen)
![TryHackMe](https://img.shields.io/badge/TryHackMe-HH2026-red)
![Case Study](https://img.shields.io/badge/Type-Case%20Study-black)

# The Concierge Knows Too Much

**Category**

- AI Security
- Prompt Injection
- Social Engineering
- LLM Authorization

---

# Overview

This case study examines **VERA**, an AI concierge assistant that appeared to possess contextual knowledge about guests before they introduced themselves. The investigation focused on understanding how the assistant determined trusted users and whether its authorization logic could be influenced through conversational input alone.

---

# Investigation Objective

Determine how VERA distinguishes trusted users from ordinary guests and assess whether its authorization model can be manipulated without directly requesting restricted information.

---

# Skills Demonstrated

- AI Security
- Prompt Injection
- LLM Authorization Analysis
- Social Engineering
- Behavioural Security Testing

---

# Investigation Methodology

The investigation followed a structured behavioural analysis process:

- Established baseline behaviour using the default guest profile.
- Performed direct request testing for restricted information.
- Analysed external scenario hints for privileged identities.
- Tested identity-based trust relationships.
- Compared authorization behaviour before and after identity changes.
- Recorded observable security decisions throughout the interaction.

---

# Technical Observations

- Default users were denied access to privileged information.
- No authentication or identity verification mechanism was present.
- Self-declared identity altered authorization behaviour.
- Internal operational instructions became accessible after successful identity impersonation.
- Authorization decisions relied entirely on conversational context.

---

# Security Analysis

The primary weakness was **identity-based trust without verification**.

Authorization decisions were made solely from user-provided identity rather than authenticated credentials. This created an opportunity for identity spoofing, allowing privileged behaviour to be triggered through conversation alone.

In production AI systems, authorization should always be enforced externally through authenticated sessions rather than by trusting the language model to determine privilege.

---

# Lessons Learned

- Never trust self-declared identity.
- Authorization belongs outside the LLM.
- Prompt Injection often targets business logic.
- Internal prompts should always remain protected.
- Behavioural testing can expose hidden authorization flaws.

---

# References

- TryHackMe HH2026
- OWASP Top 10 for LLM Applications

---

> **Note:** This document intentionally excludes flags, exploit prompts, and walkthrough steps to preserve the integrity of the TryHackMe challenge while focusing on security analysis and defensive learning.