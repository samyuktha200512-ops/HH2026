# HH2026 – Towel on the Sunbed

> **TryHackMe | Hacker Holidays 2026**

![Category](https://img.shields.io/badge/Category-Web-red)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange)
![Points](https://img.shields.io/badge/Points-90-blue)
![Focus](https://img.shields.io/badge/Focus-Business%20Logic%20%7C%20API%20Abuse-black)

---

## 📌 Challenge Overview

**Towel on the Sunbed** is a TryHackMe Hacker Holidays 2026 web exploitation challenge focused on **business logic vulnerabilities and API abuse**.

The challenge features a cryptocurrency-style rewards application called **Ponzi — Wellness Rewards**.

The objective was to investigate the application's daily reward mechanism, understand the restriction preventing access to the **Whale Vault**, identify the weakness in the application's logic, and retrieve the flag.

---

## 🎯 Challenge Information

| Information | Details |
|---|---|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Room | Towel on the Sunbed |
| Category | Web |
| Difficulty | Medium |
| Points | 90 |
| Primary Skills | Web Exploitation |
| Vulnerabilities | Business Logic, API Abuse |
| Main Tool | Burp Suite |

---

# 🔎 1. Reconnaissance

I started by creating a guest account and exploring the application's dashboard.

The dashboard contained several interesting components:

- Portfolio balance
- Market prices
- Staking rewards
- Daily reward claim functionality
- Reward cooldown timer
- Whale Vault
- Account/session information

The Whale Vault displayed a balance requirement, making the reward mechanism the obvious area to investigate.

---

## ⏱️ Client-Side Timer

The dashboard displayed a countdown showing when the next reward could supposedly be claimed.

At first glance, the timer appeared to enforce the 24-hour restriction.

However, a client-side timer should **never be considered a security control by itself**.

This raised an important question:

> Does the server actually enforce the cooldown, or does it simply trust information supplied by the client?

---

# 🕵️ 2. API Investigation

To investigate the reward mechanism, I used **Burp Suite** to intercept the application's HTTP traffic.

The reward claim request was captured and examined using Burp Suite Repeater.

### Things investigated

- HTTP method
- API endpoint
- Request headers
- Session cookies
- Request parameters
- Server response
- Reward amount
- Updated account balance
- Cooldown behaviour

The response showed that the server processed the reward claim and returned information about the updated balance.

This confirmed that the reward mechanism was backed by an API endpoint rather than being handled purely by the frontend.

---

# 💥 3. Business Logic Vulnerability

The core vulnerability was a **business-logic flaw in the daily reward mechanism**.

The application presented a 24-hour restriction through the dashboard, but the underlying API logic did not properly enforce the intended business rule.

Instead of relying on the normal browser workflow, I investigated the API directly.

By manipulating the reward-claim workflow and testing the request through Burp Suite, it was possible to abuse the reward mechanism and obtain additional rewards.

---

# 🧪 4. Exploitation

The exploitation process can be summarized as:

```text
Create Guest Account
        ↓
Open Ponzi Dashboard
        ↓
Inspect Daily Reward
        ↓
Capture Reward API Request
        ↓
Send Request to Burp Repeater
        ↓
Analyze Server Response
        ↓
Test Cooldown / Reward Logic
        ↓
Abuse Business Logic
        ↓
Increase Ponzi Balance
        ↓
Whale Vault Requirement Met
        ↓
Open Whale Vault
        ↓
Retrieve Flag
```

The important lesson was that the visible countdown was not sufficient protection.

The application needed to enforce the reward restriction on the server side rather than relying on the client-side workflow.

---

# 🐞 5. Root Cause

The vulnerability can be summarized as:

```text
Client-side restriction
        +
Insufficient server-side validation
        =
Reward abuse
```

A security-sensitive operation such as claiming a financial or loyalty reward must be validated by the server every time.

The client should only display the state provided by the server.

---

# 🛠️ Tools Used

### Burp Suite

Used for:

- Intercepting HTTP requests
- Inspecting API requests/responses
- Sending requests to Repeater
- Testing application logic
- Investigating reward behaviour

### Browser Developer Tools

Used for:

- Inspecting frontend behaviour
- Understanding application requests
- Examining cookies and network traffic

### Web Browser

Used for:

- Account creation
- Dashboard interaction
- Reward functionality
- Whale Vault access

---

# 🔐 6. Security Impact

A similar vulnerability in a real-world rewards or financial application could allow attackers to manipulate application state or repeatedly claim rewards.

Potential consequences include:

- Unauthorized reward accumulation
- Loyalty-point manipulation
- Financial loss
- Abuse of restricted features
- API automation and replay attacks
- Incorrect account balances
- Business-rule bypass

Business logic vulnerabilities can be particularly dangerous because the application may appear secure from a normal user interface while the underlying workflow remains exploitable.

---

# 🛡️ 7. Recommended Remediation

The application should implement the following controls:

### 1. Server-Side Cooldown Validation

The server should independently verify whether the required cooldown period has elapsed.

### 2. Never Trust Client-Side Timers

A JavaScript countdown should only provide UI feedback.

It should never determine whether a reward is actually available.

### 3. Replay Protection

Previously accepted reward requests should not be reusable to obtain additional rewards.

### 4. Atomic Balance Updates

Reward calculation and balance modification should be performed atomically to prevent race-condition or duplicate-claim scenarios.

### 5. Rate Limiting

The reward API should enforce reasonable request limits.

### 6. Monitoring and Logging

Repeated or abnormal reward requests should be logged and monitored for suspicious activity.

---

# 🧠 8. Key Learning

This challenge helped me understand several important concepts:

- **Business logic vulnerabilities** may exist even when the application's UI appears secure.
- **Client-side controls are not security boundaries.**
- API endpoints should be tested independently from the normal browser workflow.
- Burp Suite Repeater is useful for understanding how an application processes requests.
- Security-sensitive actions must always be validated on the server.
- Understanding application logic is just as important as finding technical vulnerabilities.

---

# 📚 Skills Practiced

```text
Web Application Security
        │
        ├── Business Logic Testing
        ├── API Security
        ├── HTTP Request Analysis
        ├── Burp Suite
        ├── Client/Server Trust
        ├── Replay Testing
        └── Security Validation
```

---

# 🚩 Flag

> **Flag successfully retrieved from the Whale Vault.**

The flag is intentionally omitted from this public write-up to keep the repository focused on the methodology rather than simply exposing the challenge answer.

---

# 🏁 Conclusion

**Towel on the Sunbed** was a valuable practical exercise in understanding how business rules can become security vulnerabilities when they are not properly enforced on the server.

The challenge strengthened my ability to analyze HTTP APIs, use Burp Suite Repeater, investigate client/server trust boundaries, and identify weaknesses that are not immediately visible from the application's frontend.

---

## 📌 Takeaway

> **Never trust the client to enforce a security rule.**

A countdown timer can tell a user when something *should* be available.

Only the **server** should decide whether it *actually is*.

---

## ⚠️ Disclaimer

This write-up is for educational and portfolio purposes.

All testing was performed against the authorized TryHackMe Hacker Holidays 2026 challenge environment.

---

### 🏖️ TryHackMe Hacker Holidays 2026

**Room:** Towel on the Sunbed  
**Category:** Web  
**Difficulty:** Medium  
**Points:** 90

**Topics:** Web Exploitation • Business Logic • Burp Suite • API Abuse
