# Yellow RAT Investigation – Network Traffic Analysis of Malicious Redirection Activity

## Cyber Defender

## Scenario

During a routine security assessment at GlobalTech Industries, unusual outbound network traffic was detected originating from multiple employee workstations. Further investigation revealed that user search queries were being redirected to unfamiliar and potentially malicious websites.

These findings raised concerns about a possible widespread compromise involving remote access malware.

As the assigned analyst, your objectives are to:

- Analyze network traffic to identify abnormal communication patterns
- Detect redirection mechanisms affecting user activity
- Identify potential Command-and-Control (C2) communication
- Determine the scope of infected systems
- Extract Indicators of Compromise (IOCs)
- Assess the presence and behavior of a Remote Access Trojan (RAT)

The goal is to uncover the attack method, identify affected systems, and support containment efforts.

---

## Alert

**Security Alert: Suspicious Network Activity & Browser Redirection Detected**

Multiple endpoints are exhibiting abnormal outbound traffic patterns, including redirection of user search queries to unknown external websites. The behavior suggests potential compromise involving malware capable of manipulating network traffic and user activity.

Indicators point toward unauthorized communication with external infrastructure, possibly linked to a Remote Access Trojan (RAT). Immediate investigation is required to identify the source, scope, and impact of the activity. Tools Used

---

## Tools Used

- VirusTotal
- Red Canary

## Given Files

```jsx
malware hash: 30E527E45F50D2BA82865C5679A6FA998EE0A1755361AB01673950810D071C85
```

## Challenge Questions

1. Understanding the adversary helps defend against attacks. What is the name of the malware family that causes abnormal network traffic?

Yellow Cockatoo is our name for a cluster of activity involving the execution of a .NET remote access trojan (RAT) that runs in memory and drops other payloads. We’ve been tracking this threat since June 2020. Yellow Cockatoo has targeted a range of victims across multiple industries and company sizes, and we continue to see it, as recently as this week.

<img width="919" height="427" alt="Screenshot 2026-05-06 224111" src="https://github.com/user-attachments/assets/3d622dce-048d-48d0-b607-e1614a3bf372" />

Answer: Yellow Cockatoo RAT

---

2.As part of our incident response, knowing common filenames the malware uses can help scan other workstations for potential infection. What is the common filename associated with the malware discovered on our workstations?

Use the platform hybrid Analysis .

Refer Link: https://hybrid-analysis.com/sample/30e527e45f50d2ba82865c5679a6fa998ee0a1755361ab01673950810d071c85/5f87b2920788cb226f59d611

```jsx
Filename:  111bc461-1ca8-43c6-97ed-911e0e69fdf8.dll
Size:  68KiB (69632 bytes)
Type:  pedll executable
Description:  PE32 executable (DLL) (console) Intel 80386 Mono/.Net assembly, for MS Windows
Architecture:  WINDOWS
SHA256: 30e527e45f50d2ba82865c5679a6fa998ee0a1755361ab01673950810d071c85
Compiler/Packer: Microsoft visual C# v7.0 / Basic .NET
```

<img width="452" height="196" alt="Screenshot 2026-05-07 145024" src="https://github.com/user-attachments/assets/71f8ae3e-81f7-473c-8df9-4850d47e90d7" />

Answer: 111bc461-1ca8-43c6-97ed-911e0e69fdf8.dll

---

3. Determining the compilation timestamp of malware can reveal insights into its development and deployment timeline. What is the compilation timestamp of the malware that infected our network?

<img width="365" height="122" alt="Screenshot 2026-05-07 145718" src="https://github.com/user-attachments/assets/1695ebc7-4275-4d0a-8de6-4e641958e8e9" />

Answer: 2020-09-24 18:26

---

4.Understanding when the broader cybersecurity community first identified the malware could help determine how long the malware might have been in the environment before detection. When was the malware first submitted to VirusTotal?

<img width="236" height="88" alt="Screenshot 2026-05-07 145837" src="https://github.com/user-attachments/assets/0ea8493e-45a5-4593-8290-c8061e8031dd" />

Answer: 2020-10-15 02:47

---

5.To completely eradicate the threat from Industries' systems, we need to identify all components dropped by the malware. What is the name of the **.dat** file that the malware dropped in the **AppData** folder?

Search for the AppData in the hybrid Analysis Report.

```jsx
Full path: \AppData\Roaming\solarmarker.dat
```

<img width="463" height="197" alt="Screenshot 2026-05-07 150403" src="https://github.com/user-attachments/assets/ad0582c3-305e-4fb8-98e0-8220e6e9a984" />

Answer: solarmarker.dat

---

6.It is crucial to identify the C2 servers with which the malware communicates to block its communication and prevent further data exfiltration. What is the C2 server that the malware is communicating with?
Use the Joe Sandbox report of the malware

Refer Link: https://www.joesandbox.com/analysis/1387728/0/html

<img width="356" height="124" alt="Screenshot 2026-05-07 151257" src="https://github.com/user-attachments/assets/5dc9f06f-a9c1-40dd-b7f8-f85a4646e2c7" />

Answer: hxxps[://]gogohid[.]comdef

---

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | - | - |
| Execution | T1055 | Process Injection |
| Execution | T1218 | System Binary Proxy Execution |
| Persistence | T1547 | Boot or Logon Autostart Execution |
| Privilege Escalation | T1055 | Process Injection |
| Defense Impairment | T1027 | Obfuscated Files or Information |
| Credential Access | - | - |
| Discovery | T1082 | System Information Discovery |
| Discovery | T1016 | System Network Configuration Discovery |
| Discovery | T1083 | File and Directory Discovery |
| Lateral Movement | - | - |
| Collection | T1005 | Data from Local System |
| Command and Control | T1071.001 | Application Layer Protocol: Web Protocols |
| Command and Control | T1105 | Ingress Tool Transfer |
| Command and Control | T1573 | Encrypted Channel |
| Exfiltration | T1041 | Exfiltration Over C2 Channel |
| Impact | - | - |

## Author

### RUTHRAN-SEC



## Author

### RUTHRAN-SEC
