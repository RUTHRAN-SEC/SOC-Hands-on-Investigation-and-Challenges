# 3CX Supply Chain Attack Investigation – Threat Intelligence & Campaign Analysis

## Cyber Defender

## Scenario

A multinational organization relies heavily on the **3CX Desktop App** for internal and external communication. Following a recent software update, antivirus alerts flagged unusual behavior, including the unexpected removal of application components on certain endpoints.

Initially dismissed as false positives, the issue escalated when employees began experiencing degraded system performance and irregular application behavior. Further investigation revealed suspicious outbound network connections to unknown external servers, raising concerns of a potential compromise.

As a Threat Intelligence Analyst, your objectives are to:

- Investigate the possibility of a supply chain attack
- Determine how the 3CX application was compromised
- Identify malicious components introduced during the update
- Analyze network communication patterns and C2 infrastructure
- Correlate findings with known threat actors and campaigns
- Assess the scope and impact of the compromise

The goal is to uncover the full attack chain and provide actionable intelligence for containment and remediation.

## Alert

**Security Alert: Suspicious Activity Linked to Software Update Deployment**

Unusual behavior has been detected across multiple endpoints following a recent update to a widely used communication application. Indicators include inconsistent application behavior, unauthorized file modifications, and abnormal outbound network traffic to previously unseen external servers.

The activity suggests a potential compromise of the software update mechanism, possibly introducing malicious code into trusted applications. Immediate investigation is required to determine the source, scope, and impact of the incident.

## Tools Used

- msi info
- VirusTotal

## Given File

```jsx
100156 -rw-rw-r-- 1 root root 102555648 May 16  2024 3CXDesktopApp-18.12.416.msi

The Hash value 
SHA256: 59e1edf4d82fae4978e97512b0331b7eb21dd4b838b850ba46794d9c7a2c0983
```

## Challenge Questions

1.Understanding the scope of the attack and identifying which versions exhibit malicious behavior is crucial for making informed decisions if these compromised versions are present in the organization. How many versions of 3CX **running on Windows** have been flagged as malware?

Refer Link: https://blogs.vmware.com/security/2023/03/investigating-3cx-desktop-application-attacks-what-you-need-to-know.html

Reports of malicious code associated with the 3CX desktop application – part of the 3CX VoIP (Voice over Internet Protocol) platform – began on March 22, 2023. On March 30, 2023, 3CX confirmed the compromise, noting the affected 3CX desktop app versions were 18.12.407 and 18.12.416 for Windows and 18. 11.1213, 18.12.402, 18.12.407 and 18.12.416 versions for Mac. NIST National Vulnerability Database has assigned CVE-2023-29059 to track this issue.

<img width="1406" height="425" alt="image" src="https://github.com/user-attachments/assets/9d8da876-0952-4287-85f8-74b6499c2594" />

3CX DesktopApp through 18.12.416 has embedded malicious code, as exploited in the wild in March 2023. This affects versions 18.12.407 and 18.12.416 of the 3CX DesktopApp Electron Windows application shipped in Update 7, and versions 18.11.1213, 18.12.402, 18.12.407, and 18.12.416 of the 3CX DesktopApp Electron macOS application.

Answer: 2

---

2.Determining the age of the malware can help assess the extent of the compromise and track the evolution of malware families and variants. What's the UTC creation time of the **`.msi`** malware?

```jsx
file 3CXDesktopApp-18.12.416.msi 

3CXDesktopApp-18.12.416.msi: Composite Document File V2 Document, 
Little Endian, Os: Windows, 
Version 6.2, MSI Installer, 
Code page: 1252, Title: Installation Database, Subject: 3CX Desktop App, 
Author: 3CX Ltd., 
Keywords: Installer, 
Comments: Windows Installer Package, 
Template: x64;1033, 
Revision Number: {99BD84FA-1803-4DA0-A416-65D94F4D208A}, 
Create Time/Date: Mon Mar 13 06:33:26 2023, 
Last Saved Time/Date: Mon Mar 13 06:33:26 2023, 
Number of Pages: 405, 
Number of Words: 2, 
Name of Creating Application: Windows Installer XML Toolset (3.11.2.4516), Security: 2

```

Answer: 2023-03-13 06:33

---

3.Executable files (**`.exe`**) are frequently used as primary or secondary malware payloads, while dynamic link libraries (**`.dll`**) often load malicious code or enhance malware functionality. Analyzing files deposited by the Microsoft Software Installer (**`.msi`**) is crucial for identifying malicious files and investigating their full potential. Which malicious DLLs were dropped by the **`.msi`** file?

On the Virus Total platform the behavior section contains the dropped file extension by the .msi file.

<img width="538" height="315" alt="Screenshot 2026-05-09 192839" src="https://github.com/user-attachments/assets/c7088b40-49df-4553-9ab9-030352498880" />

```jsx
C:\Users\user\AppData\Local\Programs\3CXDesktopApp\app-18.12.416\d3dcompiler_47.dll
C:\Users\user\AppData\Local\Programs\3CXDesktopApp\app-18.12.416\ffmpeg.dll
```

Answer: ffmpeg.dll,d3dcompiler_47.dll

---

4.Recognizing the persistence techniques used in this incident is essential for current mitigation strategies and future defense improvements. What is the MITRE Technique ID employed by the **`.msi`** files to load the malicious DLL?

Searched the dropped file name on the MITRE ATT&CK to find the persistence technique used by the malware. 

Refer Link: https://attack.mitre.org/campaigns/C0057/

#### Hijack Execution Flow: DLL

During the 3CX Supply Chain Attack, AppleJeus splits functionally across multiple .dll files using export functions, such as DLLGetClassObject, to execute code from an embedded .dll file within another .dll file. AppleJeus has also used DLL search order hijacking via the IKEEXT service, running with LocalSystem privileges, to load the TAXHAUL DLL for persistence.

<img width="640" height="314" alt="Screenshot 2026-05-09 194745" src="https://github.com/user-attachments/assets/82e961ff-e765-41b4-9d18-ed65436ce923" />

Answer: T1574

---

5.Recognizing the malware type (**`threat category`**) is essential to your investigation, as it can offer valuable insight into the possible malicious actions you'll be examining. What is the threat category of the two malicious DLLs?

To find the category of the .dll files, I took the md5 hash value form the Joe SandBox report of the malware.

Joe Sandbox Report: https://www.joesandbox.com/analysis/1710353/0/html

The MD5 hash value of d3dcompiler_47.dll

```jsx
MD5: 82187AD3F0C6C225E2FBA0C867280CC9  
```

Pasted the hash on the virustotal and found that the malware threat category was trojan.

#### Threat categories: trojan

<img width="560" height="316" alt="Screenshot 2026-05-09 195402" src="https://github.com/user-attachments/assets/cc2b3200-3ef4-4e79-810c-705f263ddf45" />

Answer: trojan

---

6.As a threat intelligence analyst conducting dynamic analysis, it's vital to understand how malware can evade detection in virtualized environments or analysis systems. This knowledge will help you effectively mitigate or address these evasive tactics. What is the MITRE ID for the virtualization/sandbox evasion techniques used by the two malicious DLLs?

To find out what MITRE ATT&CK technique used by the two DLLs we have to use the SHA256 hash values.

```jsx
d3dcompiler_47.dll: 11be1803e2e307b647a8a7e02d128335c448ff741bf06bf52b332e0bbf423b03
ffmpeg.dll: 7986BBAEE8940DA11CE089383521AB420C443AB7B15ED42AED91FD31CE833896
```

In the Virus total check out the Behavior section to see the MITRE ATT&CK technique used for virtualization/sandbox evasion.

On the stealth section both the DLLs are using the same technique

<img width="574" height="242" alt="Screenshot 2026-05-09 200252" src="https://github.com/user-attachments/assets/8c859b53-ca90-4c91-b2f6-feb8de52b470" />

Adversaries may employ various means to detect and avoid virtualization and analysis environments. This may include changing behaviors based on the results of checks for the presence of artifacts indicative of a virtual machine environment (VME) or sandbox. If the adversary detects a VME, they may alter their malware to disengage from the victim or conceal the core functions of the implant. They may also search for VME artifacts before dropping secondary or additional payloads. Adversaries may use the information learned from Virtualization/Sandbox Evasion during automated discovery to shape follow-on behaviors.

Answer: T1497

---

7.When conducting malware analysis and reverse engineering, understanding anti-analysis techniques is vital to avoid wasting time. Which hypervisor is targeted by the anti-analysis techniques in the **`ffmpeg.dll`** file?

In the Virus total use the hash value of the ffmpeg.dll to find the anti-analysis techniques used.

<img width="379" height="203" alt="Screenshot 2026-05-09 202430" src="https://github.com/user-attachments/assets/e44d7159-f290-40c2-b195-2d7d3f1a99d2" />

**Reference anti-VM strings targeting VMWare**

Answer: VMware

---

8.Identifying the cryptographic method used in malware is crucial for understanding the techniques employed to bypass defense mechanisms and execute its functions fully. What encryption algorithm is used by the **`ffmpeg.dll`** file?

<img width="365" height="263" alt="Screenshot 2026-05-09 202740" src="https://github.com/user-attachments/assets/5a3bd82f-b99c-4502-9022-f00e9683f2e2" />

Encryption algorithm is used by the **`ffmpeg.dll`** file.

- Encrypt Data: RC4
- Encryption Key: RC4 KSA

RC4 (Rivest Cipher 4) is a fast, symmetric stream cipher . It uses a variable key size (40-128 bits) to generate a pseudo-random keystream, which is XORed with plaintext to produce ciphertext. Due to severe security vulnerabilities, including key recovery attacks, RC4 is no longer considered secure and is deprecated for use in TLS.

Answer: RC4

---

9.As an analyst, you've recognized some TTPs involved in the incident, but identifying the APT group responsible will help you search for their usual TTPs and uncover other potential malicious activities. Which group is responsible for this attack?

The attacker group was the AppleJeus, searched more about **AppleJeus** in the MITRE ATT&CK to find which group was responsible for this attack.

AppleJeus is a North Korean state-sponsored threat group attributed to the Reconnaissance General Bureau. Associated with the broader Lazarus Group umbrella of actors, AppleJeus has been active since at least 2018 and is closely aligned in resources with TEMP.hermit, another DPRK-affiliated group under the same umbrella. The group’s primary mission is to generate and launder revenue to provide financial support to the government. AppleJeus primarily targets the cryptocurrency industry and is most notably responsible for the 3CX Supply Chain Attack.The group traditionally deploys malicious cryptocurrency software in combination with Phishing. From these compromised environments, it selectively deploys additional backdoors to enable extended operations against high-value financial targets.

<img width="311" height="233" alt="Screenshot 2026-05-09 203826" src="https://github.com/user-attachments/assets/660d58cb-8acb-4fbe-8789-d8bed8d3ac77" />

Answer: Lazarus

---

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | T1587 | Develop Capabilities |
| Initial Access | T1195 | Supply Chain Compromise |
| Execution | T1059 | Command and Scripting Interpreter |
| Persistence | T1574 | Hijack Execution Flow |
| Privilege Escalation | T1574 | Hijack Execution Flow |
| Defense Impairment | T1497 | Virtualization/Sandbox Evasion |
| Credential Access | - | - |
| Discovery | T1497 | Virtualization/Sandbox Detection |
| Lateral Movement | - | - |
| Collection | - | - |
| Command and Control | T1071 | Application Layer Protocol (Encrypted C2 via DLL) |
| Exfiltration | - | - |
| Impact | - | - |

## Author

### RUTHRAN-SEC
