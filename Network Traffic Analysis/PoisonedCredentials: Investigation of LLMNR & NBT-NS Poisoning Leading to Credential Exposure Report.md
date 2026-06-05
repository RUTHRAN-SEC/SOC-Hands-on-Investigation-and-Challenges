# PoisonedCredentials: Investigation of LLMNR & NBT-NS Poisoning Leading to Credential Exposure Report

## CyberDefender

## Scenario

The organization’s Security Operations Center (SOC) detected a significant increase in suspicious internal network activity. Preliminary findings suggested abnormal name resolution traffic and unusual authentication attempts across multiple hosts.

There are concerns that attackers may be exploiting **LLMNR (Link-Local Multicast Name Resolution)** and **NBT-NS (NetBIOS Name Service)** protocols to perform poisoning attacks within the internal network.

Such attacks can allow adversaries to:

- Intercept broadcast name resolution requests
- Impersonate legitimate hosts
- Capture NTLM authentication hashes
- Potentially crack credentials offline

You have been tasked with analyzing network logs and packet captures to:

- Confirm whether LLMNR/NBT-NS poisoning occurred
- Identify the attacking host
- Determine which credentials were exposed
- Assess the potential impact
- Provide mitigation recommendations

## Alert

**SOC Alert: Suspicious Name Resolution Traffic Detected**

Security monitoring systems identified:

- High volume of LLMNR and NBT-NS broadcast traffic
- Multiple unsolicited name resolution responses
- Suspicious NTLM authentication exchanges
- Potential credential leakage events

The activity pattern is consistent with known credential interception techniques used in internal network attacks.

Severity: **High – Potential Credential Compromise**

## Tools used

- Wireshark

## Challenge Question

1.In the context of the incident described in the scenario, the attacker initiated their actions by taking advantage of benign network traffic from legitimate machines. Can you identify the specific mistyped query made by the machine with the IP address 192.168.232.162?
The filter used:

```jsx
ip.src == 192.168.232.162
```

On the packet

```jsx
47	74.354273	192.168.232.162	192.168.235.255	NBNS	92	Name query NB FILESHAARE<20>
```

<img width="960" height="422" alt="Screenshot 2026-03-29 190657" src="https://github.com/user-attachments/assets/97dc696b-cdbe-4dab-8d16-5dbd4d9e36b3" />

The user add mistyped the word “fileshaare” instead of “fileshare”.

Answer: fileshaare

---

2.We are investigating a network security incident. To conduct a thorough investigation, We need to determine the IP address of the rogue machine. What is the IP address of the machine acting as the rogue entity?

<img width="960" height="421" alt="Screenshot 2026-03-29 190809" src="https://github.com/user-attachments/assets/f7c439c5-b480-44e3-8a08-41433a24a0b6" />

The IP address that responded to query was “192.168.232.215”

Answer: 192.168.232.215

---

3.As part of our investigation, identifying all affected machines is essential. What is the IP address of the second machine that received poisoned responses from the rogue machine?
We are using the rogue machine IP address which is “192.168.232.215”.

The filter used:

```jsx
ip.addr==192.168.232.215
```

On the packet:

```jsx
51	74.355657	192.168.232.215	192.168.232.162	NBNS	104	Name query response NB 192.168.232.215
```

Where “192.168.232.162” was received the second poisoned response.

<img width="960" height="418" alt="Screenshot 2026-03-29 191435" src="https://github.com/user-attachments/assets/c66731ba-28b7-4121-9c67-bff48bb5291a" />

Answer: 192.168.232.176

---

4.We suspect that user accounts may have been compromised. To assess this, we must determine the username associated with the compromised account. What is the username of the account that the attacker compromised?

The system is compromised, So we filter it out with the smb2

```jsx
smb2
```

On the packet 

```jsx
242	398.476497	192.168.232.215	192.168.232.176	SMB2	598	Session Setup Request, NTLMSSP_AUTH, User: cybercactus.local\janesmith
```

<img width="960" height="422" alt="Screenshot 2026-03-29 192331" src="https://github.com/user-attachments/assets/5243a24e-21b1-4fc3-bc95-a3d9d59fc8cd" />

Answer: janesmith

---

5.As part of our investigation, we aim to understand the extent of the attacker's activities. What is the hostname of the machine that the attacker accessed via SMB?

On the packet 

```jsx
242	398.476497	192.168.232.215	192.168.232.176	SMB2	598	Session Setup Request, NTLMSSP_AUTH, User: cybercactus.local\janesmith
```

<img width="821" height="475" alt="Screenshot 2026-03-29 193628" src="https://github.com/user-attachments/assets/c1cfa352-8b16-4624-8aa0-dc19663f894e" />

Answer: AccountingPC

---
# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1557.001 | LLMNR/NBT-NS Poisoning and SMB Relay |
| Execution | - | - |
| Persistence | - | - |
| Privilege Escalation | - | - |
| Defense Impairment | - | - |
| Credential Access | T1557.001, T1110.002 | LLMNR/NBT-NS Poisoning and SMB Relay, Password Cracking |
| Discovery | - | - |
| Lateral Movement | T1021.002 | SMB/Windows Admin Shares |
| Collection | - | - |
| Command and Control | - | - |
| Exfiltration | - | - |
| Impact | - | - |

## Author

### RUTHRAN-SEC
