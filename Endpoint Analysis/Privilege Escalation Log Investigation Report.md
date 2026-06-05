# **Log Analysis: Privilege Escalation and Unauthorized Root Access Investigation**

## Blue Team Lab Online

## Scenario

A Linux server hosting a PHP-based website was compromised, resulting in sensitive data being leaked to an underground forum.

The exposed data was restricted to the **root account**, raising concerns about privilege escalation.

Initial findings indicate:

- Remote access typically logs in as `www-data`
- The `www-data` user does not have permission to access the sensitive files
- The developer claims proper filtering prevents malicious PHP file uploads
- Provided bash history does not clearly show malicious commands

Your objective is to:

- Determine how the attacker escalated privileges
- Identify how root-level access was obtained
- Analyze system artifacts for privilege abuse
- Establish a timeline of compromise

## Alert

**Critical Privilege Escalation Alert:** Unauthorized root-level access suspected on a production web server following remote access under the `www-data` account.

Sensitive data restricted to root privileges was exfiltrated and published externally, indicating potential privilege escalation and post-exploitation activity.

Immediate forensic investigation required to determine escalation vector and scope of compromise.

---

## Given Files:

- bash_history

## Challenge Questions

1.What user (other than ‘root’) is present on the server?

When Analyzing the Bash_history file. The user other then root was “Daniel”. Where the attacker moved the directory using command

```jsx
cd /home/daniel/
```

<img width="950" height="412" alt="Screenshot 2026-02-28 112157" src="https://github.com/user-attachments/assets/866e26aa-833a-483d-a996-b97930901497" />

Answer: daniel

---

2.What script did the attacker try to download to the server? 

The attacker used wget tool to download exploit file “linux-exploit-suggester.sh”. Linux-exploit-suggester is a security auditing tool designed for penetration testers and security analysts to quickly identify potential local Linux privilege escalation vulnerabilities.

<img width="960" height="389" alt="Screenshot 2026-02-28 112614" src="https://github.com/user-attachments/assets/9c308b09-f07a-49cf-a64e-96d8d6b6ed29" />

Answer: linux-exploit-suggester[ . ]sh

---

3.What packet analyzer tool did the attacker try to use? 

The attacker used tcpdump packet analyzer tool.Tcpdump is a powerful command-line packet analyzer used to capture, filter, and inspect network traffic flowing to and from a computer. It is primarily used by system administrators and security professionals to troubleshoot network connectivity issues, monitor performance, and analyze security threats. 

<img width="960" height="395" alt="Screenshot 2026-02-28 113031" src="https://github.com/user-attachments/assets/c6861408-132f-4cca-848e-c28cde8dc913" />

Answer: tcpdump

---

4.What file extension did the attacker use to bypass the file upload filter implemented by the developer? 

The attacker used .phtml extension to bypass the upload filter. A .phtml file is a web file extension that stands for "PHP HTML" or "PHP and HTML." It is used for files that contain a mixture of HTML markup and PHP scripting code.

<img width="927" height="356" alt="Screenshot 2026-02-28 113318" src="https://github.com/user-attachments/assets/b5e35a78-30b1-4161-b92f-10d0d695a74a" />

Answer: .phtml

---

5.Based on the commands run by the attacker before removing the php shell, what misconfiguration was exploited in the ‘python’ binary to gain root-level access? 1- Reverse Shell ; 2- File Upload ; 3- File Write ; 4- SUID ; 5- Library load.

Option 4 cause SUID (Set User ID) is a special Linux file permission allowing executable files to run with the privileges of the file's owner rather than the user running it

### Why no for other options.

- Option 1: There is no commands like netcat or reverse shell commands in the bash history.
- Option 2: The attacker uploaded a x.phtml file after gaining access to the root.
- Option 3: The attacker did not write up any file cause there is no nano or sign of editing the files.
- Option 5: Attacker did not load any library like .os in it.

Answer: Option 4

---

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | T1083 | File and Directory Discovery |
| Resource Development | - | - |
| Initial Access | T1190 | Exploit Public-Facing Application |
| Execution | - | - |
| Persistence | - | - |
| Privilege Escalation | T1548.001 | Setuid and Setgid |
| Defense Impairment | - | - |
| Credential Access | - | - |
| Discovery | T1046 | Network Service Discovery |
| Lateral Movement | - | - |
| Collection | - | - |
| Command and Control | - | - |
| Exfiltration | - | - |
| Impact | - | - |

### DONE BY

#### RUTHRAN-SEC
