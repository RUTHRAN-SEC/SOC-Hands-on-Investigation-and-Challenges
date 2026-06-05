# PsExec Lateral Movement Investigation – SMB Traffic & Network Forensics Analysis

## Cyber Defender

## Scenario

An Intrusion Detection System (IDS) generated an alert indicating suspicious lateral movement activity within the enterprise environment. Preliminary findings suggest the attacker leveraged **PsExec** to execute commands remotely and move between systems using SMB.

A PCAP file containing the captured network traffic has been provided for forensic investigation.

As the assigned SOC Analyst, your objectives are to:

- Analyze SMB traffic for signs of PsExec activity
- Identify the initial compromised host (entry point)
- Trace attacker movement across systems
- Detect compromised user accounts and credentials
- Identify accessed administrative shares (ADMIN$, C$, IPC$)
- Determine the extent of the compromise and attacker objectives

The goal is to reconstruct the attacker’s lateral movement and provide actionable findings for containment and remediation.

## Alert

**IDS Alert: Suspicious PsExec Lateral Movement Detected**

Network monitoring systems identified abnormal SMB activity consistent with remote command execution and lateral movement techniques. Traffic analysis indicates possible use of PsExec to access multiple hosts within the environment.

Observed indicators include:

- SMB connections to administrative shares
- Remote service creation activity
- Suspicious authentication attempts
- Unauthorized remote command execution

Immediate investigation is required to identify compromised systems, trace attacker movement, and assess the scope of the breach.

## Tools used

- WireShark

## Given Files

```jsx
9796 -rw-r--r-- 1 root root 10027488 Oct 14  2023 psexec-hunt.pcapng
```

## Challenges Questions

1.To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

In order to find the machine IP address, We wan to Analysis the top talker in the list of conversation.

And find what was the attacker IP and how did he gained access.

<img width="526" height="124" alt="Screenshot 2026-05-09 125449" src="https://github.com/user-attachments/assets/fd50a567-e499-4254-a2c7-1f8563753c8e" />

Also Found that the packet was mostly SMB protocol, So we can analysis the top talker with the SMB protocol to find the machine IP address.

<img width="503" height="197" alt="Screenshot 2026-05-09 125819" src="https://github.com/user-attachments/assets/423aa121-572d-4f7b-9d0d-9e240abce4dc" />

The Top talker was “**10.0.0.130**” ****and in the wireshark i have filtered to see only the SMB2 protocols alone and found that the IP **10.0.0.130** made a connection to the IP **10.0.0.133** where that can be the attacker IP address but not yet confirmed.

**10.0.0.130** was the machine IP address.

Answer: **10.0.0.130**

---

2.To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?
I have searched in the google to find the filter for finding the target name.

ntlmssp.challenge.target_name is a field in the NTLMv2 Type 2 "Challenge" message. It **indicates the server's authentication target, such as a NetBIOS domain name, DNS domain name, or computer name**. This data is often used in NTLM relay and information disclosure attacks to identify internal infrastructure. 
On the packet

```jsx
131	2023-10-11 07:42:08.878607443	10.0.0.133	10.0.0.130	SMB2	329	Session Setup Response, Error: STATUS_MORE_PROCESSING_REQUIRED, NTLMSSP_CHALLENGE
```

<img width="640" height="230" alt="Screenshot 2026-05-09 141439" src="https://github.com/user-attachments/assets/eb581e48-c01c-40f4-81a0-763bed00cca0" />

#### Target Name: SALES-PC

Answer: SALES-PC

---

3.Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?
Used the Filter: ntlmssp 

On the packet

```jsx
132	2023-10-11 07:42:08.879115750	10.0.0.130	10.0.0.133	SMB2	595	Session Setup Request, NTLMSSP_AUTH, User: \ssales
```

<img width="640" height="272" alt="Screenshot 2026-05-09 142150" src="https://github.com/user-attachments/assets/b7f7e4f2-0ebf-46f5-911d-e0508b1c6363" />

#### User name: ssales

Answer: ssales

---

4.After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?
Filtered the packets using the IP address of the attacker

On the packet 

```jsx
38546	2023-10-11 07:46:21.980762272	10.0.0.131	10.0.0.130	SMB2	410	Create Response File: PSEXESVC.exe
```

<img width="625" height="125" alt="Screenshot 2026-05-09 144813" src="https://github.com/user-attachments/assets/fff038db-282a-49d2-b208-66abae5d212c" />

Answer: PSEXESVC.exe

---

5.We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?
Filtered the packets using the IP address of the attacker to find the response of the target.

On the packet 

```jsx
38641	2023-10-11 07:46:21.983245793	10.0.0.131	10.0.0.130	SMB2	138	Write Response
```

<img width="640" height="245" alt="Screenshot 2026-05-09 145611" src="https://github.com/user-attachments/assets/cecf344f-f632-49cd-96c1-8da88b22be1d" />

#### Tree Id: 0x00000001  \\10.0.0.131\ADMIN$

Answer: ADMIN$

---

6.We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?
Filtered the packets using the IP address of the attacker

On the packet

```jsx
38772	2023-10-11 07:46:22.299334357	10.0.0.131	10.0.0.130	SMB2	138	Tree Connect Response
```

<img width="632" height="224" alt="Screenshot 2026-05-09 150501" src="https://github.com/user-attachments/assets/554ffd2c-0a42-43b4-a928-a0ea1e5bb348" />

#### Tree Id: 0x00000005  \\10.0.0.131\IPC$

Answer: IPC$

---

7.Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?
Filtered the packets using the `ntlmssp.challenge.target_name`

On the packet

```jsx
38514	2023-10-11 07:46:19.911313715	10.0.0.131	10.0.0.130	SMB2	369	Session Setup Response, Error: STATUS_MORE_PROCESSING_REQUIRED, NTLMSSP_CHALLENGE
```

<img width="639" height="244" alt="Screenshot 2026-05-09 153331" src="https://github.com/user-attachments/assets/94659b90-8166-4ea5-953d-8f658d99c488" />

#### Target Name: MARKETING-PC

Answer: Marketing-PC

---

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | - | - |
| Execution | T1569.002 | System Services (PsExec) |
| Persistence | T1543.003 | Windows Service |
| Privilege Escalation | - | - |
| Defense Impairment | - | - |
| Credential Access | T1550.002 | Use of Valid Accounts (Pass-the-Hash / NTLM Authentication) |
| Discovery | - | - |
| Lateral Movement | T1021.002 | SMB/Windows Admin Shares (PsExec) |
| Collection | - | - |
| Command and Control | - | - |
| Exfiltration | - | - |
| Impact | - | - |

## Author

### RUTHRAN-SEC
