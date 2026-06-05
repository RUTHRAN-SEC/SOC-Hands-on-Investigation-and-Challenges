# Operationalizing MITRE ATT&CK – Threat Intelligence & Detection Use-Case Analysis

## Blue Team Lab Online

## Scenario

As a Blue Team member within a cybersecurity team, you are tasked with enhancing the organization’s threat detection and response capabilities using the MITRE ATT&CK framework.

You are provided with multiple scenario-based security incidents and are required to:

- Map observed behaviors to MITRE ATT&CK tactics and techniques
- Identify potential attack paths used by adversaries
- Translate threat intelligence into actionable detection logic
- Recommend defensive strategies based on adversary behavior

The objective is to operationalize the MITRE ATT&CK framework by transforming theoretical knowledge into practical detection and response capabilities.

## Alert

**Threat Intelligence Tasking: Adversary Behavior Mapping Using MITRE ATT&CK**

Security operations require improved visibility into adversary techniques and attack patterns. Recent incidents highlight gaps in detection capabilities due to lack of structured threat mapping.

The team has been tasked with leveraging the MITRE ATT&CK framework to:

- Identify adversary tactics and techniques
- Correlate observed activity with known attack patterns
- Develop actionable detection and response strategies

Immediate analysis is required to strengthen defensive posture and improve threat detection accuracy.

## Tools Used

- MITRE ATT&Ck
- Google Search Engine

## Challenge Questions

1.Your company heavily relies on cloud services like Azure AD, and Office 365 publicly. What technique should you focus on mitigating, to prevent an attacker performing Discovery activities if they have obtained valid credentials?

We are using the MITRE ATT&CK Framework to find out the what is the technique ID for the Discovery Activities also in the questions they have mentioned that the company is based on the cloud service. If have to search accordingly to the cloud discovery technique.

<img width="959" height="478" alt="Screenshot 2026-05-03 130924" src="https://github.com/user-attachments/assets/5b20ab84-bfd1-4bb6-a64e-210218766221" />

Under the Discovery look at the technique Cloud service Dashboard.

<img width="762" height="473" alt="Screenshot 2026-05-03 131041" src="https://github.com/user-attachments/assets/d05f1c32-ff69-4224-a273-1a04fb421646" />

An adversary may use a cloud service dashboard GUI with stolen credentials to gain useful information from an operational cloud environment, such as specific services, resources, and features. For example, the GCP Command Center can be used to view all assets, review findings of potential security risks, and run additional queries, such as finding public IP addresses and open ports.

Answer: T1538

---

2.You were analyzing a log and found uncommon data flow on port 4050. What APT group might this be? 

In the MITRE ATT&CK search bar, look for the port “4050” and check on the Non-Standard Port for the APT group ID.

In the Non-Standard Port Section search for the port “4050” by Ctrl+f to search.

<img width="960" height="474" alt="Screenshot 2026-05-03 133211" src="https://github.com/user-attachments/assets/e66866c3-3a48-450f-ac63-38de37bc9de8" />

APT-C-36 has used port 4050 for C2 communications.

APT-C-36 is a suspected South American threat group that has engaged in espionage and financially motivated operations since at least 2018. APT-C-36 has targeted government institutions and entities in the financial, energy, and professional manufacturing sectors across Colombia and other Latin American countries.

Answer: G0099 

---

3.The framework has a list of 9 techniques that falls under the tactic to try to get into your network. What is the tactic ID? 

Search for “get into your network” in search bar. It was the in Initial Access.

<img width="960" height="477" alt="Screenshot 2026-05-03 134907" src="https://github.com/user-attachments/assets/77ceff17-6603-4eb7-8f1d-26ba9b9e2ac8" />

The adversary is trying to get into your network.

Initial Access consists of techniques that use various entry vectors to gain their initial foothold within a network. Techniques used to gain a foothold include targeted spearphishing and exploiting weaknesses on public-facing web servers. Footholds gained through initial access may allow for continued access, like valid accounts and use of external remote services, or may be limited-use due to changing passwords.

Answer: TA0001

---

4.A software prohibits users from accessing their account by deleting, locking the user account, changing password etc. What such software has been documented by the framework? 

In the MITRE ATT&CK framework the APT-C-36 check for the software name in Software Section that is used with the user account.

Refer link: https://attack.mitre.org/groups/G0099/

Search for User account and found out the software name that is been used.

<img width="770" height="474" alt="Screenshot 2026-05-03 135756" src="https://github.com/user-attachments/assets/8efffd80-6145-4fab-8a58-e15b44efca83" />

Remcos is a closed-source tool that is marketed as a remote control and surveillance software by a company called Breaking Security. Remcos has been observed being used in malware campaigns.

Answer: S0332

---

5.Using ‘Pass the Hash’ technique to enter and control remote systems on a network is common. How would you detect it in your company?

What is a Pass-the-Hash Attack?

A Pass-the-Hash (PtH) attack is a cyberattack where an attacker steals a hashed user password (typically NTLM) from a compromised system's memory and uses it to authenticate to other network resources, without ever needing the original plaintext password. It is a critical lateral movement technique, often targeting Windows environments to gain unauthorized access.

<img width="960" height="475" alt="Screenshot 2026-05-03 142118" src="https://github.com/user-attachments/assets/2c9e680a-2d22-4007-88ee-9faa01009bdb" />

Used Google Dorking to search for the Detection Used.

Refer link: https://mitre.ptsecurity.com/en-US/T1550.002

Monitor newly created logons and credentials used in events and review for discrepancies. Unusual remote logins that correlate with other suspicious activity (such as writing and executing binaries) may indicate malicious activity.

Answer: Monitor newly created logons and credentials used in events and review for discrepancies

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | TA0001 | Initial Access |
| Execution | - | - |
| Persistence | - | - |
| Privilege Escalation | - | - |
| Defense Impairment | - | - |
| Credential Access | T1550.002 | Pass the Hash |
| Discovery | T1538 | Cloud Service Dashboard |
| Lateral Movement | T1550.002 | Pass the Hash |
| Collection | - | - |
| Command and Control | T1571 | Non-Standard Port |
| Exfiltration | - | - |
| Impact | T1531 | Account Access Removal |

## Author

### RUTHRAN-SEC
