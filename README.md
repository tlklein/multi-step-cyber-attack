![image](banner.png)

# Multi-Step Cyber Attack Simulation (TryHackMe: Bookstore CTF)

## Project Overview

This project emulates the tactics, techniques, and procedures (TTPs) used by real-world adversaries to compromise an organization. Conducted in a controlled environment on [TryHackMe](https://tryhackme.com), the Bookstore CTF challenged our team to perform reconnaissance, exploit vulnerabilities, escalate privileges, and capture root access.

The lab was divided into two parts:

1. Practical CTF Simulation – Boot-to-root challenge with service enumeration, REST API fuzzing, file inclusion exploits, and privilege escalation.
2. Analytical Lab Report – Documentation of methodology, tools, findings, and reflections on the simulation.

This hands-on project strengthened my skills in penetration testing, exploit development, incident reporting, and teamwork under time constraints.

## Team Members

Victor Lopez - Contributor
Ryker Workman - Contributor
Edwin Kurian - Contributor
Amber Hood - Contributor
Trinity Klein - Contributor

## Tools & Technologies

- RustScan – port scanning & reconnaissance
- GoBuster – directory brute forcing
- cURL & Fuzzing – API endpoint discovery & exploitation
- Netcat (nc) – reverse shell listener
- Ghidra – binary decompilation & reverse engineering
- Linux CLI – privilege escalation & system enumeration

## Attack Methodology

1. Reconnaissance – Scanned target VM → discovered ports `22 (SSH), 80 (HTTP), 5000 (API)`.
2. Web Enumeration – Identified vulnerable API endpoint (v1) via assets page & comments.
3. File Inclusion Exploit – Used fuzzing to retrieve `.bash_history`, extracting debugger PIN.
4. Werkzeug Debugger Exploit – Gained shell access via Python reverse shell payload.
5. Privilege Escalation – Analyzed custom binary (`try-harder`) with Ghidra, identified required “magic number.”
6. Root Access – Derived correct value, executed binary, escalated to root, and retrieved `root.txt`.

## Key Learning

- Importance of systematic reconnaissance in penetration testing.
- Applied REST API fuzzing to exploit insecure endpoints.
- Hands-on experience with reverse engineering binaries for privilege escalation.
- Strengthened incident reporting & documentation skills.
- Practiced team-based problem solving under strict time constraints.

## Outcome

Successfully achieved root access on the Bookstore CTF machine, demonstrating end-to-end penetration testing workflow from reconnaissance to privilege escalation.

## Repository Structure

```
/multi-step-cyber-attack
│── README.md              # Project summary
│── report.pdf             # Full lab report PDF
│── notes/commands.txt     # Step-by-step commands and notes
│── screenshots/           # Screenshots walking though lab
```

## References

1. Stapler 1 - CTF Walkthrough - Boot-To-Root 
a. Link: https://www.youtube.com/watch?v=cSOAzEQHlh0   
2. CTF Walkthrough with John Hammond 
a. Link: https://youtu.be/ZUqGSbvZp1k?si=ONyPaIZz48_W5fAc   
3. Hacking Bookstore 
a. Link: https://www.youtube.com/watch?v=47S9DyA3Z6Y&ab_channel=elbee  
4. Hacking APIs: Fuzzing 101 
a. https://www.youtube.com/watch?v=47S9DyA3Z6Y&ab_channel=elbeehttps://w
ww.youtube.com/watch?v=M_guA0wjrLg  
5. TryHackMe! Bookstore - REST API Fuzzing //walk-through 
a. Link: https://www.youtube.com/watch?v=un6aNIbpGUY&ab_channel=Yesspider  
6. Bookstore — TryHackMe — WriteUp  
a. Link: https://tonyrahmos.medium.com/bookstore-tryhackme-writeup-
a2e87e6064f1  

---

Note: This simulation was conducted in a controlled TryHackMe lab environment with no real-world systems harmed.