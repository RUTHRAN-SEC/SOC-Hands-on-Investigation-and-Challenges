# UFO-1: Threat Intelligence Analysis of Sandworm (BlackEnergy / APT44) Targeting ICS Environments

## HackTheBox

## Scenario

As a Threat Intelligence intern in an Industrial Control Systems (ICS) environment, you have been tasked with researching advanced threat actors targeting critical infrastructure sectors.

Your assignment focuses on the threat group known as Sandworm Team, also referred to as BlackEnergy Group or APT44. This group is known for conducting sophisticated cyber operations against energy, government, and industrial sectors.

Your objectives are to:

- Research the history and operations of the threat actor
- Identify tools, malware families, and attack patterns
- Map adversary tactics, techniques, and procedures (TTPs) using MITRE ATT&CK
- Understand how attacks impact ICS environments
- Translate intelligence into actionable defensive insights

The goal is to strengthen organizational awareness and improve detection capabilities against advanced threats.

## Alert

**Threat Intelligence Advisory: Sandworm Activity Targeting ICS Environments**

Threat intelligence sources indicate ongoing activity from a highly sophisticated APT group known as Sandworm (APT44), historically linked to disruptive cyber operations against critical infrastructure.

Key concerns include:

- Targeting of energy and industrial control systems
- Use of destructive malware and ICS-specific attack techniques
- Advanced persistence and lateral movement capabilities
- Alignment with geopolitical objectives

Organizations operating in ICS environments are at elevated risk and must align detection strategies with known adversary TTPs.

Severity: **Critical – Nation-State Threat Activity**

## Tools Used

- Google
- MITRE ATT&CK

## Challenge Questions

1.According to the sources cited by Mitre, in what year did the Sandworm Team begin operations?

Sandworm Team is a destructive threat group that has been attributed to Russia's General Staff Main Intelligence Directorate (GRU) Main Center for Special Technologies (GTsST) military unit 74455. This group has been active since at least 2009.

<img width="960" height="471" alt="Screenshot 2026-04-24 134156" src="https://github.com/user-attachments/assets/3ce6cfaa-57ce-40e7-9666-9ed62984fef1" />

Answer: 2009

---

2.Mitre notes two credential access techniques used by the BlackEnergy group to access several hosts in the compromised network during a 2016 campaign against the Ukrainian electric power grid. One is LSASS Memory access (T1003.001). What is the Attack ID for the other?

Search for the 2016 campaign against the Ukrainian electric power grid.

Refer Link: https://attack.mitre.org/campaigns/C0025/

<img width="959" height="471" alt="Screenshot 2026-04-24 160854" src="https://github.com/user-attachments/assets/86093a5f-2533-424b-9aa1-9744e8ff0dff" />

Account Manipulation, Brute Force, Command and Scripting Interpreter: Visual Basic, Command and Scripting Interpreter: PowerShell, Command and Scripting Interpreter: Windows Command Shell, Command-Line Interface, Compromise Host Software Binary, Create Account, Create Account: Domain Account, Create or Modify System Process: Windows Service, Impair Defenses: Disable Windows Event Logging, Lateral Tool Transfer, Lateral Tool Transfer, Masquerading: Masquerade File Type, Masquerading: Match Legitimate Resource Name or Location, Masquerading: Masquerade Account Name, Masquerading, Obfuscated Files or Information, Obfuscated Files or Information: Software Packing, OS Credential Dumping: LSASS Memory, Remote Services: SMB/Windows Admin Shares, Remote Services, Remote System Discovery, Scripting, Server Software Component: SQL Stored Procedures, Valid Accounts, Windows Management Instrumentation.

Go to the attack navigator bar. And search for the credential access techniques.

<img width="960" height="470" alt="Screenshot 2026-04-24 162048" src="https://github.com/user-attachments/assets/a71d934a-ab3b-4e21-a505-8cde8947632f" />

Adversaries may use brute force techniques to gain access to accounts when passwords are unknown or when password hashes are obtained. Without knowledge of the password for an account or set of accounts, an adversary may systematically guess the password using a repetitive or iterative mechanism. Brute forcing passwords can take place via interaction with a service that will check the validity of those credentials or offline against previously acquired credential data, such as password hashes.

Answer: T1110

---

3.During the 2016 campaign, the adversary was observed using a VBS script during their operations. What is the name of the VBS file?

Refer Link: https://attack.mitre.org/campaigns/C0025/

<img width="960" height="467" alt="Screenshot 2026-04-24 162428" src="https://github.com/user-attachments/assets/3bde7f4e-994f-400c-add9-5e4f2369ca4c" />

During the 2016 Ukraine Electric Power Attack, Sandworm Team used a VBS script to facilitate lateral tool transfer. The VBS script was used to copy ICS-specific payloads with the following command: `cscript C:\Backinfo\ufn.vbs C:\Backinfo\101.dll C:\Delta\101.dll`

Answer: ufn.vbs

---

4.The APT conducted a major campaign in 2022. The server application was abused to maintain persistence. What is the Mitre Att&ck ID for the persistence technique was used by the group to allow them remote access?

Refer Link: 

[2022 Ukraine Electric Power Attack, Campaign C0034 | MITRE ATT&CK®](https://attack.mitre.org/campaigns/C0034/)

Search for the campaign happened in 2022.

<img width="958" height="472" alt="Screenshot 2026-04-24 162725" src="https://github.com/user-attachments/assets/7b59cbae-3d6a-4765-9c56-aa7814ba4657" />

Go to the attack navigator bar. And search for the credential access techniques.

<img width="956" height="463" alt="Screenshot 2026-04-24 163203" src="https://github.com/user-attachments/assets/04bd542b-4482-4a2a-b13e-c6fd2b40fc74" />

Adversaries may backdoor web servers with web shells to establish persistent access to systems. A Web shell is a Web script that is placed on an openly accessible Web server to allow an adversary to access the Web server as a gateway into a network. A Web shell may provide a set of functions to execute or a command-line interface on the system that hosts the Web server.

Answer: T1505.003

---

5.What is the name of the malware / tool used in question 4?

In the technique search for the ID T1505.003.

Refer Link: https://attack.mitre.org/techniques/T1505/003/

<img width="960" height="471" alt="Screenshot 2026-04-25 120249" src="https://github.com/user-attachments/assets/835f3759-1852-4899-a826-7c885e4312ea" />

Neo-reGeorg is an open-source web shell designed as a restructuring of reGeorg with improved usability, security, and fixes for exising reGeorg bugs.

Answer: Neo-REGEORG 

---

6.Which SCADA application binary was abused by the group to achieve code execution on SCADA Systems in the same campaign in 2022?

Search for the word SCADA.

The 2022 Ukraine Electric Power Attack was a Sandworm Team campaign that used a combination of GOGETTER, Neo-REGEORG, CaddyWiper, and living of the land (LotL) techniques to gain access to a Ukrainian electric utility to send unauthorized commands from their SCADA system.

<img width="960" height="473" alt="Screenshot 2026-04-25 120624" src="https://github.com/user-attachments/assets/dd406c94-269f-40c4-8812-f2cfe5dbab18" />

During the 2022 Ukraine Electric Power Attack, Sandworm Team executed a MicroSCADA application binary `scilc.exe` to send a predefined list of SCADA instructions specified in a file defined by the adversary, `s1.txt`. The executed command `C:\sc\prog\exec\scilc.exe -do pack\scil\s1.txt` leverages the SCADA software to send unauthorized command messages to remote substations.

Answer: scilc.exe

---

7.Identify the full command line associated with the execution of the tool from question 6 to perform actions against substations in the SCADA environment.

During the 2022 Ukraine Electric Power Attack, Sandworm Team executed a MicroSCADA application binary `scilc.exe` to send a predefined list of SCADA instructions specified in a file defined by the adversary, `s1.txt`. The executed command `C:\sc\prog\exec\scilc.exe -do pack\scil\s1.txt` leverages the SCADA software to send unauthorized command messages to remote substations.

Answer: C:\sc\prog\exec\scilc.exe -do pack\scil\s1.txt

---

8.What malware/tool was used to carry out data destruction in a compromised environment during the same campaign?

During the 2022 Ukraine Electric Power Attack, Sandworm Team deployed CaddyWiper on the victim’s IT environment systems to wipe files related to the OT capabilities, along with mapped drives, and physical drive partitions.

<img width="957" height="476" alt="Screenshot 2026-04-25 143500" src="https://github.com/user-attachments/assets/1ff91474-ae38-436c-9916-2f84c8e84874" />

Answer: CaddyWiper

---

9.The malware/tool identified in question 8 also had additional capabilities. What is the Mitre Att&ck ID of the specific technique it could perform in Execution tactic?

Refer Link: https://attack.mitre.org/software/S0693/

Move to the ATT&CK navigator.

Refer Link: https://mitre-attack.github.io/attack-navigator//#layerURL=https%3A%2F%2Fattack.mitre.org%2Fsoftware%2FS0693%2FS0693-enterprise-layer.json

<img width="960" height="463" alt="Screenshot 2026-04-25 144148" src="https://github.com/user-attachments/assets/68c1680b-d51e-4e41-9490-71f900c79c03" />

Adversaries may interact with the native OS application programming interface (API) to execute behaviors. Native APIs provide a controlled means of calling low-level OS services within the kernel, such as those involving hardware/devices, memory, and processes. These native APIs are leveraged by the OS during system boot (when other system components are not yet initialized) as well as carrying out tasks and requests during routine operations.

Answer: T1106

---

10.The Sandworm Team is known to use different tools in their campaigns. They are associated with an auto-spreading malware that acted as a ransomware while having worm-like features. What is the name of this malware?

In October 2020, the US indicted six GRU Unit 74455 officers associated with Sandworm Team for the following cyber operations: the 2015 and 2016 attacks against Ukrainian electrical companies and government organizations, the 2017 worldwide `NotPetya` attack, targeting of the 2017 French presidential campaign, the 2018 Olympic Destroyer attack against the Winter Olympic Games, the 2018 operation against the Organisation for the Prohibition of Chemical Weapons, and attacks against the country of Georgia in 2018 and 2019. Some of these were conducted with the assistance of GRU Unit 26165, which is also referred to as APT28.

<img width="960" height="470" alt="Screenshot 2026-04-25 144854" src="https://github.com/user-attachments/assets/7fa409cd-51ce-49dc-9b5b-f9824463d7f6" />

Answer: NotPetya

---

11.What was the Microsoft security bulletin ID for the vulnerability that the malware from question 10 used to spread around the world?

Search in google for “"notpetya" ms security bulletin”

<img width="960" height="471" alt="Screenshot 2026-04-25 145529" src="https://github.com/user-attachments/assets/3595a535-c679-4d91-a916-b5a67248c008" />

Refer LInk: https://www.microsoft.com/en-us/security/blog/2018/02/05/overview-of-petya-a-rapid-cyberattack/

<img width="960" height="471" alt="Screenshot 2026-04-25 145757" src="https://github.com/user-attachments/assets/993d2e00-73a1-42bb-8e2c-21196f6d5b1c" />

**Traverse** – The malware used two means to traverse:

- *Exploitation* – Exploited vulnerability in SMBv1 (MS17-010).
- *Credential theft* – Impersonated any currently logged on accounts (including service accounts).
- Note that Petya only compromised accounts that were logged on with an active session (e.g. credentials loaded into LSASS memory).

Answer: MS17-010

---

12.What is the name of the malware/tool used by the group to target modems?

In google search:

- Sandworm Team Tool used for "modems" attack site:attack.mitre.org

<img width="958" height="473" alt="Screenshot 2026-04-25 150319" src="https://github.com/user-attachments/assets/36b9151e-d9c0-4cf9-b604-fb7e1882f184" />

AcidRain is an ELF binary targeting modems and routers using MIPS architecture. AcidRain is associated with the ViaSat KA-SAT communication outage that took place during the initial phases of the 2022 full-scale invasion of Ukraine. Analysis indicates overlap with another network device-targeting malware, VPNFilter, associated with Sandworm Team. US and European government sources linked AcidRain to Russian government entities, while Ukrainian government sources linked AcidRain specifically to Sandworm Team.

Answer: AcidRain 

---

13.Threat Actors also use non-standard ports across their infrastructure for Operational-Security purposes. On which port did the Sandworm team reportedly establish their SSH server for listening?

Refer Link: https://attack.mitre.org/groups/G0034/

<img width="960" height="473" alt="Screenshot 2026-04-25 150605" src="https://github.com/user-attachments/assets/9edfe754-3aa6-4027-91b4-af5445a59122" />

Sandworm Team has used port 6789 to accept connections on the group's SSH server.

Answer: 6789

---

14.The Sandworm Team has been assisted by another APT group on various operations. Which specific group is known to have collaborated with them?

the 2018 operation against the Organization for the Prohibition of Chemical Weapons, and attacks against the country of Georgia in 2018 and 2019. Some of these were conducted with the assistance of GRU Unit 26165, which is also referred to as APT28.

<img width="960" height="476" alt="Screenshot 2026-04-25 151038" src="https://github.com/user-attachments/assets/22e4686a-64f8-46ae-9217-493296738ec8" />

Answer: APT28

---

<img width="524" height="178" alt="Screenshot 2026-04-25 151109" src="https://github.com/user-attachments/assets/61a4bb48-0e49-4f1a-8bd0-cd987f882dcd" />

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1110 | Brute Force |
| Execution | T1059.005 | Visual Basic |
| Execution | T1059.001 | PowerShell |
| Execution | T1059.003 | Windows Command Shell |
| Execution | T1106 | Native API |
| Persistence | T1505.003 | Web Shell |
| Persistence | T1543.003 | Windows Service |
| Privilege Escalation | T1078 | Valid Accounts |
| Defense Impairment | T1562.001 | Disable or Modify Tools |
| Credential Access | T1003.001 | LSASS Memory |
| Credential Access | T1110 | Brute Force |
| Discovery | T1018 | Remote System Discovery |
| Lateral Movement | T1021.002 | SMB/Windows Admin Shares |
| Lateral Movement | T1570 | Lateral Tool Transfer |
| Collection | - | - |
| Command and Control | T1090 | Proxy |
| Exfiltration | - | - |
| Impact | T1485 | Data Destruction |

## Author

### RUTHRAN-SEC
