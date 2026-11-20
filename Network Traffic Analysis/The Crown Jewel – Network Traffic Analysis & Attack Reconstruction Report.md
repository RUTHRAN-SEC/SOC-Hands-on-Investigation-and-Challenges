# The Crown Jewel – Network Traffic Analysis & Attack Reconstruction

## TryHackMe

## **Scenario**

A suspicious incident has raised concerns about a potential insider threat within an organization. Investigators have obtained a network packet capture (PCAP) and several files that may contain hidden information related to the incident.

The investigation requires analyzing network communications, recovering password-protected files, examining image metadata, and extracting concealed information to identify the individual responsible.

As the assigned investigator, your objectives are to:

- Analyze the PCAP file and identify relevant response messages.
- Recover the password-protected ZIP archive.
- Examine file metadata for suspicious information.
- Identify hidden data embedded within image files.
- Extract concealed information using steganography techniques.
- Identify the attacker's profile and uncover the insider's identity.

The goal is to correlate digital evidence and uncover the individual behind the suspicious activity.

## **Alert**

**Insider Threat Alert: Suspicious Network Communication & Concealed Data Detected**

Investigators identified suspicious network communications and potentially concealed information within files associated with an internal security incident.

Initial evidence includes a PCAP file, password-protected archives, and image files containing potentially hidden information.

Immediate forensic investigation is required to analyze the network evidence, extract concealed data, and identify the individual responsible for the incident.

## Tools Used

- Splunk
- Wireshark

## Given Files

```jsx
splunk - network_logs
challenge.pcap
```

## Challenge Questions

1.From which internal IP did the suspicious connection originate?

```jsx
index=network_logs log_type=ids
```

On the IDS tool generated the alert on the “**Reverse Shell Outbound Connection Detected**” 

<img width="640" height="296" alt="Screenshot 2026-11-19 160658" src="https://github.com/user-attachments/assets/6ddcde4b-9a47-4e14-ab64-03d2180b4c29" />

```jsx
index=network_logs 
| stats earliest(_time) as Ear
| convert ctime(Ear)

index=network_logs 
| stats latest(_time) as latest
| convert ctime(latest)
```

The earliest Event was 11/01/2025 01:03:32.836577 And the latest Event was 11/20/2025 09:43:54.612357, There was a 20 days gap between the incident happened and the investgation in started. There where totally 5061 Events.

Answer: 10.10.10.100

---

2.What outbound connection was detected as a C2 channel?

```jsx
index=network_logs "1.1.1.1" 
```

Investigated the 1.1.1.1 ip address of the dns resolver on the port 8080 where the used for the C2 using the known CVE-202X-EXPLOIT as a agent , the C2 through the RCE on http protocol.

<img width="640" height="296" alt="Screenshot 2026-11-19 212849" src="https://github.com/user-attachments/assets/4bdbfde4-ff96-4f6f-9ed4-6c20b00808b7" />

Answer: 1.1.1.1:8080

---

3.Which MAC address is impersonating the gateway 10.10.10.1? 

On the ARP log types there the is the mac address fields contains the impersonated ip address of the gateway ip.

```jsx
index=network_logs "10.10.10.1" log_type=arp
```

Answer: 00:0c:29:11:22:33

---

4.What is the non-standard User-Agent hitting the Jira instance?

```jsx
index=network_logs "jira" 
| stats count by event.agent
```

<img width="639" height="296" alt="Screenshot 2026-11-20 143608" src="https://github.com/user-attachments/assets/ca80cfba-a360-43f1-a49b-1a10085b2a7e" />

As the user agent CVE-202X-EXPLOIT seem to be the suspicious used to exploit to the taarget.

Answer:  CVE-202X-EXPLOIT   

---

5.How many ARP spoofing attacks were observed in the PCAP?

```jsx
index=network_logs "jira" log_type="arp"
```

<img width="640" height="293" alt="Screenshot 2026-11-20 144011" src="https://github.com/user-attachments/assets/8da66b6f-edeb-4a3d-bd78-ae6f04dc4dc6" />

Answer: 90 

---

6.What's the payload containing the plaintext creds found in the POST request?

Can’t find username and password text in splunk , so i moved Wireshark for viewing 

```jsx
http.request.method == POST
```

```jsx
POST /login.php HTTP/1.1
Host: internal.login.local
Content-Length: 42
Content-Type: application/x-www-form-urlencoded

username=dev_user&password=SecretPassword!
```

<img width="454" height="272" alt="Screenshot 2026-11-20 215254" src="https://github.com/user-attachments/assets/c8447b33-416b-4d9e-9024-0d6bde5865d5" />

Answer: username=dev_user&password=SecretPassword!

---

7.What domain, owned by the attacker, was used for data exfiltration?

For investigating the domain name we have to look for the dns_server logs for the domain name used for the data exfiltration.

```jsx
index=network_logs log_type="dns"
| stats count by event.domain
```

<img width="640" height="295" alt="Screenshot 2026-11-20 215739" src="https://github.com/user-attachments/assets/53344e84-fc60-4fec-97ae-0fc6fedf5b96" />

the domain name was `exfil-domain.xyz` 

Answer: exfil-domain.xyz

---

8.After examining the logs, which protocol was used for data exfiltration?

The exfiltration is happened via the dns protocol   

Answer: dns

---

## **MITRE ATT&CK Mapping**

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance |  |  |
| Resource Development |  |  |
| Initial Access |  |  |
| Execution |  |  |
| Persistence |  |  |
| Privilege Escalation |  |  |
| Defense Impairment |  |  |
| Credential Access |  |  |
| Discovery |  |  |
| Lateral Movement |  |  |
| Collection |  |  |
| Command and Control | **T1071.001** | Application Layer Protocol: Web Protocols |
| Exfiltration | **T1048** | Exfiltration Over Alternative Protocol |
| Impact |  |  |

### **Evidence supporting the mapping**

- **Command and Control — T1071.001 (Web Protocols)**

The investigation identified an outbound connection to `1.1.1.1:8080`, described as C2 communication using HTTP and an RCE exploit agent.

- **Exfiltration — T1048 (Exfiltration Over Alternative Protocol)**

Your investigation identified `exfil-domain.xyz` as the attacker-owned domain and DNS as the protocol used for data exfiltration.

---

### DONE BY

#### RUTHRAN-SEC
