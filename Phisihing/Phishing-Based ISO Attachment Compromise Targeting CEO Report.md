# Phishing-Based ISO Attachment Compromise Targeting CEO 

## **Boogeyman 3 CTF**

## TryHackMe(Challenge)

### Tool Used: Elastic Stack (ELK)

### Scenario

Quick Logistics LLC was targeted by the Boogeyman threat group using a stealthy phishing campaign that bypassed existing security controls.

The attackers initially compromised an employee’s email account and remained dormant to avoid detection.

Using this trusted internal access, the threat actors sent a phishing email to the CEO, Evan Hutchinson. Although the email appeared suspicious, the CEO opened the attached document, which contained a malicious ISO payload.

After opening the attachment and observing no visible activity, the CEO reported the email to the security team. During the investigation, the security team identified:

- The phishing attachment in the Downloads folder
- A suspicious file extracted from the ISO payload

The activity was believed to have occurred between August 29 and August 30, 2023, indicating a delayed or stealth execution technique used by the attackers.

The objective of this investigation is to analyze the payload, assess system impact, and identify attacker behavior.

## Alert: **Suspicious Phishing Email with Malicious ISO Attachment Opened by CEO**

The Overview content of the phishing email content 

<img width="960" height="391" alt="Screenshot 2026-01-13 161926" src="https://github.com/user-attachments/assets/d929cedc-f551-4ce6-ba3d-42e086877205" />


Moving to investigate in elastic tool. The incident occurred between August 29 and August 30, 2023 so we filter the Date & Time settings into the date of the incident to view the events.

### Challenge Questions

1.What is the PID of the process that executed the initial stage 1 payload?

We know that the file name was ProjectFinancialSummary_Q3.pdf, We search this in the field 

<img width="960" height="441" alt="Screenshot 2026-01-13 163520" src="https://github.com/user-attachments/assets/f96f2363-2202-4c64-bc76-081130da2fe2" />


Answer: 6392

2.The stage 1 payload attempted to implant a file to another location. What is the full command-line value of this execution?

Add a filed that are can be helpful in investigation like: Time, Process.command_line, Process.name, Process.parent.command_line, username.

After the the execution the next processes is what we are searching for. Sort to with the Time field to analysis it better 

<img width="960" height="437" alt="Screenshot 2026-01-13 165200" src="https://github.com/user-attachments/assets/671e29e2-f5b6-4e53-9b92-06f5dd2a1167" />

Answer: C:\Windows\System32\xcopy.exe" /s /i /e /h D:\review.dat C:\Users\EVAN~1.HUT\AppData\Local\Temp\review.dat

3.The implanted file was eventually used and executed by the stage 1 payload. What is the full command-line value of this execution?

<img width="959" height="438" alt="Screenshot 2026-01-13 165553" src="https://github.com/user-attachments/assets/e588ee2e-914b-47d0-bf0d-57db66c5c9ad" />

Answer: "C:\Windows\System32\rundll32.exe" D:\review.dat,DllRegisterServer

4.The stage 1 payload established a persistence mechanism. What is the name of the scheduled task created by the malicious script?

Check next event log where the payload established the scheduled task in that expand the document to view the commands.

<img width="960" height="404" alt="Screenshot 2026-01-13 170000" src="https://github.com/user-attachments/assets/2276ea67-ca11-41d4-bedf-5bfa408ef513" />

Answer: Review

5.The execution of the implanted file inside the machine has initiated a potential C2 connection. What is the IP and port used by this connection? (format: IP:port)

Filtering the user name with “Evan Hutchinson” and enabling the source ip/port and destination ip/port in order to see the ip and port that is established for C2 connection.  

<img width="960" height="431" alt="Screenshot 2026-01-14 130607" src="https://github.com/user-attachments/assets/5bbb2616-511a-4d1f-999e-9870ecab28d1" />

Answer: 165.232.170.151:80

6.The attacker has discovered that the current access is a local administrator. What is the name of the process used by the attacker to execute a UAC bypass?

Checking the process name by filtering using the review.dat. found out 39 hits shows different processes. checking each one , for knowing which one can bypass UAC.

```json
Process Names:
Powershell.exe - a executable file that runs PowerShell.
net.exe - When a Microsoft Windows command-line utility (Network Command) for managing network resources, users, groups, and services
whoami.exe - a legitimate Microsoft Windows command-line utility that displays user, group, and privilege information for the account currently logged on to the local system
xcopy.exe - a legitimate Microsoft Windows command-line utility that displays user, group, and privilege information for the account currently logged on to the local system
fodhelper.exe - (Features on Demand Helper) is a legitimate, trusted Microsoft Windows executable used to manage optional Windows features, such as language settings
rundll32.exe - a legitimate Windows system program that loads and runs functions from 32-bit Dynamic Link Libraries (DLLs), essential for system operations but also frequently abused by malware as a proxy to hide malicious code execution and bypass security
```

In MITRE ATT&CK under the privilege execution →  Abuse Elevation Control Mechanism. Searched for Fodhelper to see it can bypass UAC 

<img width="958" height="452" alt="Screenshot 2026-01-14 140015" src="https://github.com/user-attachments/assets/8bb34665-ef8e-4d26-a13d-eccd1337a782" />

Answer: fodhelper.exe

7.Having a high privilege machine access, the attacker attempted to dump the credentials inside the machine. What is the GitHub link used by the attacker to download a tool for credential dumping?

In order to find out the github link we have to search “github.com”

<img width="960" height="435" alt="Screenshot 2026-01-14 153217" src="https://github.com/user-attachments/assets/e1086dc2-a531-4b1b-81b5-ab15f4fb8efb" />

Answer: hxxps[://]github[.]com/gentilkiwi/mimikatz/releases/download/2[.]2[.]0-20220919/mimikatz_trunk[.]zip → the is dangerous so i defang the url using cyberchef 

8.After successfully dumping the credentials inside the machine, the attacker used the credentials to gain access to another machine. What is the username and hash of the new credential pair? (format: username:hash)

Filtering out the search field using “Mimikatz.exe” cause attacker use this process to gain access to the another machine.

<img width="960" height="435" alt="Screenshot 2026-01-14 154221" src="https://github.com/user-attachments/assets/c3395e30-cdd7-4039-8931-a40e8b5a104c" />

Answer: itadmin:F84769D250EB95EB2D7D8B4A1C5613F2

9.Using the new credentials, the attacker attempted to enumerate accessible file shares. What is the name of the file accessed by the attacker from a remote share?

Filter the field using “user.name :"evan.hutchinson" and process.name :"powershell.exe" and event.code: "1" and host.name : "WKSTN-0051.quicklogistics.org" ”

<img width="960" height="436" alt="Screenshot 2026-01-14 175421" src="https://github.com/user-attachments/assets/774e10b3-120f-4911-a621-9d1d9b3adbb5" />

Answer: IT_Automation.ps1

10.After getting the contents of the remote file, the attacker used the new credentials to move laterally. What is the new set of credentials discovered by the attacker? (format: username:password)

To find out the credentials we use → host.name :"WKSTN-0051.quicklogistics.org" and powershell.exe and *credential*  and we need process.command_line field to view the credentials 

<img width="960" height="442" alt="Screenshot 2026-01-14 201152" src="https://github.com/user-attachments/assets/93dbb034-87ff-4b17-a2b4-c9a864f9f60b" />

Answer: QUICKLOGISTICS\allan.smith:Tr!ckyP@ssw0rd987

11.What is the hostname of the attacker's target machine for its lateral movement attempt?

<img width="960" height="436" alt="Screenshot 2026-01-14 175421" src="https://github.com/user-attachments/assets/555c827f-caa7-4af6-9eb5-e0cd92f295ab" />

Answer: WKSTN-1327

12.Using the malicious command executed by the attacker from the first machine to move laterally, what is the parent process name of the malicious command executed on the second compromised machine?

I filtered the search field → host.name :"WKSTN-1327.quicklogistics.org" and event.provider : "Microsoft-Windows-Sysmon" and event.code :"1"and field we need are process.command_line, process.parent.command_line, process.parent_name.

<img width="960" height="440" alt="Screenshot 2026-01-14 203851" src="https://github.com/user-attachments/assets/727be0ba-4730-4ba3-90fe-912cedcbc4e7" />

Answer: wsmprovhost.exe

13.The attacker then dumped the hashes in this second machine. What is the username and hash of the newly dumped credentials? (format: username:hash)

I filtered with → host.name :"WKSTN-1327.quicklogistics.org" and event.provider : "Microsoft-Windows-Sysmon" and event.code :"1" and Mimikatz.exe

And the fields required are process.command_line, process.parent.command_line, user_name.

<img width="960" height="436" alt="Screenshot 2026-01-14 210339" src="https://github.com/user-attachments/assets/14dcdfe9-46f1-442c-ba1b-ac7a74322c99" />

Answer: administrator:00f80f2538dcb54e7adc715c0e7091ec

14.After gaining access to the domain controller, the attacker attempted to dump the hashes via a DCSync attack. Aside from the administrator account, what account did the attacker dump?

<img width="959" height="438" alt="Screenshot 2026-01-14 212449" src="https://github.com/user-attachments/assets/8fcc438e-62b2-458a-8d8a-d56460431319" />

Answer: backupda

15.After dumping the hashes, the attacker attempted to download another remote file to execute ransomware. What is the link used by the attacker to download the ransomware binary?

<img width="951" height="402" alt="Screenshot 2026-01-14 220305" src="https://github.com/user-attachments/assets/93378f1f-fd31-4ca0-a2b0-353bf9181f0e" />

Answer: http://ff.sillytechninja.io/ransomboogey.exe

### Done By Ruthran-sec
