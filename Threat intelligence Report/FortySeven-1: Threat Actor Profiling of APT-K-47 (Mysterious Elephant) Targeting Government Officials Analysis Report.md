# FortySeven-1: Threat Actor Profiling of APT-K-47 (Mysterious Elephant) Targeting Government Officials

## HackTheBox

## Scenario

An Advanced Persistent Threat (APT) group has been observed conducting a targeted cyber espionage campaign using Hajj-themed phishing lures to compromise government and diplomatic officials.

The attackers aim to harvest sensitive communications, particularly WhatsApp data, to support intelligence collection objectives.

Your intelligence team has gathered fragmented reporting from:

- Public cybersecurity vendor research
- Independent security blogs
- Internal security alerts

As a Threat Intelligence Analyst, your task is to:

- Correlate findings across multiple reports
- Identify the threat actor responsible
- Analyze malware families and tooling used
- Map TTPs to MITRE ATT&CK
- Assess targeting patterns and geographic focus
- Determine strategic motives
- Produce a consolidated threat profile

The objective is to transform fragmented intelligence into a structured and actionable threat assessment.

## Alert

**Threat Intelligence Advisory: Targeted Phishing Campaign Against Government & Diplomatic Entities**

Security monitoring and external reporting indicate:

- Use of Hajj-themed phishing lures
- Credential harvesting and malware delivery
- WhatsApp data exfiltration attempts
- Activity linked to an APT operating in South Asia
- Deployment of custom tools such as AsyncShell

The campaign appears highly targeted and aligned with geopolitical intelligence collection objectives.

Severity: **Critical – State-Aligned Espionage Activity**

## Tools Used

- Google Search Engine

## Given Evidence

Evidence - 1 - `https://securelist.com/mysterious-elephant-apt-ttps-and-tools/117596/` 

Evidence - 2 - `https://medium.com/@knownsec404team/apt-k-47-mysterious-elephant-a-new-apt-organization-in-south-asia-5c66f954477` 

Evidence - 3 - `https://medium.com/@knownsec404team/unveiling-the-past-and-present-of-apt-k-47-weapon-asyncshell-5a98f75c2d68`

---

## Challenge Questions

1.What is the primary name of the APT group described in the SecureList report?

<img width="960" height="468" alt="Screenshot 2026-04-21 131332" src="https://github.com/user-attachments/assets/fd240194-2e94-48fe-96f1-c28cee5c88ad" />

Answer: Mysterious Elephant

---

2.According to the Knownsec 404 team's analysis(Evidence -3), since which year has this group's attack activity been dated back to?

<img width="960" height="471" alt="Screenshot 2026-04-21 131811" src="https://github.com/user-attachments/assets/8a8891ba-9240-431b-ad4e-e41b179b08b3" />

Answer: 2022

---

3.The group uses a custom backdoor that communicates via Office Remote Procedure Call (ORPCBackdoor). According to the Knownsec 404 team's analysis(Evidence -2), what is the name of the first malicious exported entry function?

There are two malicious entries of ORPCBackdoor, the first is GetFileVersionInfoBy- HandleEx(void) export function, second place is DllEntryPoint.

<img width="959" height="470" alt="Screenshot 2026-04-21 140345" src="https://github.com/user-attachments/assets/3531dbb7-abb3-4c54-a4f5-f326bf65ec56" />

Answer: GetFileVersionInfoByHandleEx(void)

---

4.The previously mentioned backdoor checks for a file before creating persistence. What is the name of the file?

<img width="960" height="476" alt="Screenshot 2026-04-21 140640" src="https://github.com/user-attachments/assets/64bab48e-fd14-4c4e-b965-fbb9cf98e891" />

### Persistent

ORPCBackdoor determines whether the file exists to prevent multiple persistent creation. Before persistent creation, ORPCBackdoor determines whether the ts.dat file exists in the same path. If the file does not exist, ORPCBackdoor will create persistence. The TaskScheduler CLSID is invoked by COM,which name is Microsoft Update. After the task is created, the ts.dat file is created.

Answer: ts.dat

---

5.The use of the backdoor links the APT to another well-known South Asian APT group. What is the name of this other group?

Use the Search Bar tool and search for the word “South Asian”

<img width="960" height="473" alt="Screenshot 2026-04-21 141226" src="https://github.com/user-attachments/assets/ffcf7ce3-ebb7-49ed-a95f-8f4dcc17a155" />

Based on our analysis of other South Asian organizations Sidewinder, Patchwork, cnc, confucious, BITTER, and APT-K-47, we can see that these hacker organizations may be different groups under a unified organization, and there are many overlapping situations in terms of attack tools, attack targets, and network assets.

Answer: BITTER

---

6.The APT group we are currently investigating has consistently used and updated another backdoor since 2023, with its C2 communication evolving from TCP to HTTPS. What is the name of this tool?

Referred the evidence 3: https://medium.com/@knownsec404team/unveiling-the-past-and-present-of-apt-k-47-weapon-asyncshell-5a98f75c2d68

Search the word Https

<img width="875" height="474" alt="Screenshot 2026-04-21 141934" src="https://github.com/user-attachments/assets/95657e30-6854-4b17-8151-eb7952b5cda1" />

Load communication changed from tcp to https, so we noted as Asyncshell-v2.

Answer:  Asyncshell-v2

---

7.To evade sandbox analysis, the MemLoader HidenDesk tool checks the number of active processes before running. What is the minimum number of processes required for it to proceed?

Referred the evidence 1: https://securelist.com/mysterious-elephant-apt-ttps-and-tools/117596/

<img width="960" height="479" alt="Screenshot 2026-04-21 142859" src="https://github.com/user-attachments/assets/937b086f-170f-4e1a-b637-af5ffc71fd74" />

The malware checks the number of active processes and terminates itself if there are fewer than 40 processes running — a technique used to evade sandbox analysis.

Answer: 40

---

8.The MemLoader HidenDesk tool creates a covert environment for its activities by creating and switching to a specific environment. What is the name of this hidden desktop?

Referred the evidence 1: https://securelist.com/mysterious-elephant-apt-ttps-and-tools/117596/

<img width="960" height="472" alt="Screenshot 2026-04-21 151238" src="https://github.com/user-attachments/assets/50d4b746-f411-4bfd-a81c-dbdfb1d4a195" />

The malware then creates a hidden desktop named “MalwareTech_Hidden” and switches to it, providing a covert environment for its activities. This technique is borrowed from an open-source project on GitHub.

Answer: MalwareTech_Hidden

---

9.The MemLoader HidenDesk tool achieves persistence by placing a shortcut in the autostart folder to ensure it runs after a system reboot. What is the MITRE ATT&CK ID for the 'Registry Run Keys / Startup Folder' technique?

Refer : https://attack.mitre.org/techniques/T1547/001/

<img width="951" height="470" alt="Screenshot 2026-04-21 150744" src="https://github.com/user-attachments/assets/8061990e-e428-45cf-8302-2c7643eae796" />

Adversaries may achieve persistence by adding a program to a startup folder or referencing it with a Registry run key. Adding an entry to the "run keys" in the Registry or startup folder will cause the program referenced to be executed when a user logs in. These programs will be executed under the context of the user and will have the account's associated permissions level.

Answer: T1547.001

---

10.The actor uses several custom exfiltration tools targeting WhatsApp. What is the name of the tool that recursively searches specific directories, including the “Desktop” and “Downloads” folders?

Refer the evidence 1

<img width="960" height="474" alt="Screenshot 2026-04-21 151815" src="https://github.com/user-attachments/assets/faadda8c-b37e-4ed6-b307-4e99f5ed4347" />

The Stom Exfiltrator is a commonly used exfiltration tool that recursively searches specific directories, including the “Desktop” and “Downloads” folders, as well as all drives except the C drive, to collect files with predefined extensions. Its latest variant is specifically designed to target files shared through the WhatsApp application. This version uses a hardcoded folder path to locate and exfiltrate such files: The targeted file extensions include .PDF, .DOCX, .TXT, .JPG, .PNG, .ZIP, .RAR, .PPTX, .DOC, .XLS, .XLSX, .PST, and .OST.

Answer: Stom Exfiltrator

---

11.Kaspersky's analysis highlights the actor's heavy use of scripts for execution and deploying payloads. What is the MITRE ATT&CK ID for the 'PowerShell' technique?

Refer the link: https://attack.mitre.org/techniques/T1059/001/

<img width="960" height="477" alt="Screenshot 2026-04-21 152053" src="https://github.com/user-attachments/assets/b0d589ca-132e-47f5-808d-89e3c690f90f" />

Adversaries may abuse PowerShell commands and scripts for execution. PowerShell is a powerful interactive command-line interface and scripting environment included in the Windows operating system. Adversaries can use PowerShell to perform a number of actions, including discovery of information and execution of code. Examples include the `Start-Process` cmdlet which can be used to run an executable and the `Invoke-Command` cmdlet which runs a command locally or on a remote computer (though administrator permissions are required to use PowerShell to connect to remote systems).

Answer: T1059.001

---

12.In their early attack chains, Mysterious Elephant used a downloader that was previously associated with the Origami Elephant group. What was the name of this downloader?

Refer the Evidence 1

<img width="960" height="461" alt="Screenshot 2026-04-21 152330" src="https://github.com/user-attachments/assets/4e739a39-7561-4084-96f0-1ef5e2ee25e9" />

Mysterious Elephant is a threat actor we’ve been tracking since 2023. Initially, its intrusions resembled those of the Confucius threat actor. However, further analysis revealed a more complex picture. We found that Mysterious Elephant’s malware contained code from multiple APT groups, including Origami Elephant, Confucius, and SideWinder, which suggested deep collaboration and resource sharing between teams. Notably, our research indicates that the tools and code borrowed from the aforementioned APT groups were previously used by their original developers, but have since been abandoned or replaced by newer versions. However, Mysterious Elephant has not only adopted these tools, but also continued to maintain, develop, and improve them, incorporating the code into their own operations and creating new, advanced versions. The actor’s early attack chains featured distinctive elements, such as remote template injections and exploitation of CVE-2017-11882, followed by the use of a downloader called “Vtyrei”, which was previously connected to Origami Elephant and later abandoned by this group. Over time, Mysterious Elephant has continued to upgrade its tools and expanded its operations, eventually earning its designation as a previously unidentified threat actor.

Answer: Vtyrei

---

13.In a January 2024 campaign delivering an Asyncshell payload, which CVE was exploited in the malicious archive file?

Refer the Evidence 3

<img width="958" height="476" alt="Screenshot 2026-04-21 153415" src="https://github.com/user-attachments/assets/d6abfae3-ce7b-4331-8f2c-988e3ecdfa06" />

Our team first discovered Asyncshell back in January 2024, when we found a malicious sample exploiting the CVE-2023–38831 vulnerability, with the overall attack chain.

Answer: CVE-2023-38831

---

14. What is the MD5 hash of the ChromeStealer Exfiltrator sample named WhatsAppOB.exe?

Refer Evidence 1

<img width="952" height="471" alt="Screenshot 2026-04-21 153800" src="https://github.com/user-attachments/assets/fc163df2-9155-4f4d-ac78-9b7fa13c5eee" />

MD5: 9e50adb6107067ff0bab73307f5499b6 → WhatsAppOB.exe

Answer: 9e50adb6107067ff0bab73307f5499b6

---

15.The intelligence describes multiple custom tools designed to upload stolen data to the actor's servers. According to the MITRE ATT&CK framework, what is the ID for the 'Exfiltration Over C2 Channel' technique?

Refer link: https://attack.mitre.org/techniques/T1041/

<img width="960" height="474" alt="Screenshot 2026-04-21 154059" src="https://github.com/user-attachments/assets/bbed0f5e-7da7-405d-b995-a688021a6db6" />

Adversaries may steal data by exfiltrating it over an existing command and control channel. Stolen data is encoded into the normal communications channel using the same protocol as command and control communications.

Answer: T1041

---

<img width="574" height="177" alt="Screenshot 2026-04-21 154200" src="https://github.com/user-attachments/assets/3f0c6089-00fa-4298-98c3-19e89fff80d6" />

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1566.001 | Spearphishing Attachment |
| Initial Access | T1566.002 | Spearphishing Link |
| Execution | T1204.002 | User Execution: Malicious File |
| Execution | T1059.001 | PowerShell |
| Execution | T1106 | Native API |
| Persistence | T1547.001 | Registry Run Keys / Startup Folder |
| Defense Impairment | T1497.003 | Time Based Checks |
| Credential Access | - | - |
| Discovery | - | - |
| Lateral Movement | T1534 | Internal Spearphishing |
| Collection | T1005 | Data from Local System |
| Command and Control | T1071.001 | Application Layer Protocol: Web Protocols |
| Exfiltration | T1041 | Exfiltration Over C2 Channel |
| Impact | - | - |

## Author

### RUTHRAN-SEC

## Author

### RUTHRAN-SEC
