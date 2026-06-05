# Network-Based Ransomware Investigation – Traffic Analysis & File Decryption Case Study Report

## Blue Team Lab Online

## Scenario

ABC Industries was in the final stages of preparing a critical tender document when a ransomware attack disrupted operations and encrypted the final version of the file. The attack is suspected to be targeted and potentially linked to a competitor.

Due to the impact on business continuity, the incident response team collected available artifacts, including network traffic captures, the ransom note, and the encrypted document.

As the assigned analyst, your objectives are to:

- Analyze network traffic to identify ransomware behavior
- Detect communication with attacker infrastructure
- Extract possible encryption keys or artifacts from traffic
- Understand how the ransomware was delivered
- Attempt recovery or decryption of the encrypted file
- Assess the scope and impact of the attack

The goal is to restore access to the critical document and understand the attack methodology.

## Alert

**Ransomware Activity Detected: Critical File Encryption Incident**

Security monitoring identified abnormal activity within the network, followed by the sudden encryption of critical business files. A ransom note was discovered on the affected system, indicating a ransomware compromise.

Network traffic analysis suggests potential communication with external infrastructure prior to encryption, possibly involving key exchange or payload delivery. Immediate investigation is required to trace the attack, recover encrypted data, and prevent further impact.

## Tools used:

- Wireshark
- Tshark
- TCPdump

## Given files:

```jsx
  8 -rw-r--r-- 1 root root   7856 Feb 25  2021 help_recover_instructions.HTM
 88 -rw-r--r-- 1 root root  87236 Feb 25  2021 help_recover_instructions.png
  4 -rw-r--r-- 1 root root   2097 Feb 25  2021 help_recover_instructions.TXT
568 -rw-r--r-- 1 root root 576464 Feb 25  2021 ransom_traffic.pcapng
```

## Challenge Questions

1.What is the operating system of the host from which the network traffic was captured?

In Statistics Open Capture File Properties. Check out the OS Under the Capture Section 

<img width="638" height="412" alt="Screenshot 2026-05-01 164705" src="https://github.com/user-attachments/assets/ced5df06-0556-4d3f-ba81-473388930295" />

Answer: 32-bit Windows 7 Service Pack 1, build 7601

---

2.What is the full URL from which the ransomware executable was downloaded?

Filter the Wireshark using only to view “HTTP” protocols.

On the packet:

```jsx
59	2021-01-31 11:00:27.989959	10.0.2.4	10.0.2.15	HTTP	311	GET /safecrypt.exe HTTP/1.1  
```

<img width="640" height="415" alt="Screenshot 2026-05-01 175144" src="https://github.com/user-attachments/assets/9c77386f-e5ce-4f8c-84a3-d2f4ca808412" />

Check the Destination port number where the download request sent  

<img width="952" height="420" alt="Screenshot 2026-05-01 180034" src="https://github.com/user-attachments/assets/23149d4a-c1fc-4487-912a-5af815ed006f" />

Answer: http:10.0.2.15:8000/safecrypt[.]exe

---

3.Name the ransomware executable file? 

Check the name of the file in request that have been made 

```jsx
59	2021-01-31 11:00:27.989959	10.0.2.4	10.0.2.15	HTTP	311	GET /safecrypt.exe HTTP/1.1  
```

<img width="639" height="415" alt="Screenshot 2026-05-01 180246" src="https://github.com/user-attachments/assets/3a65786b-887e-4c7d-8c9f-d212c3165996" />

Answer: safecrypt.exe

---

4.What is the MD5 hash of the ransomware?

Download the safecrypt.exe from the export object→http

<img width="564" height="411" alt="Screenshot 2026-05-01 180405" src="https://github.com/user-attachments/assets/fd84e4a1-0ad2-4639-a0c8-88d72e9883b0" />

Handle safely cause mistakes can be made.

In the terminal use the command md5sum filename.exe

<img width="958" height="411" alt="Screenshot 2026-05-01 180757" src="https://github.com/user-attachments/assets/f1be6a5e-a55f-44e6-a719-831774b012ec" />

Answer: 4a1d88603b1007825a9c6b36d1e5de44

---

5.What is the name of the ransomware? 

Perform OSINT on the md5 hash value of the file.

VirusTotal Result Screen Shot

<img width="960" height="470" alt="Screenshot 2026-05-01 181220" src="https://github.com/user-attachments/assets/fc40ad9f-2b4a-4d6b-9e3a-1039a9e9d8af" />

TeslaCrypt was a notorious ransomware trojan first discovered in early 2015 that gained immediate fame for specifically targeting computer gamers. Unlike other ransomware of the time that focused on standard office documents, TeslaCrypt encrypted over 40 file types associated with popular games like Minecraft, World of Warcraft, and Call of Duty, alongside standard Word, PDF, and JPEG files. 

Answer: TeslaCrypt

---

6.What is the encryption algorithm used by the ransomware, according to the ransom note?

Check out the file “help_recover_instructions.TXT”

<img width="960" height="419" alt="Screenshot 2026-05-01 181636" src="https://github.com/user-attachments/assets/ffd1a928-1191-46ad-81f3-c0234d85cb93" />

All of your files were protected by a strong encryption with RSA-4096.

Answer:  RSA-4096

---

7.What is the domain beginning with ‘d’ that is related to ransomware traffic?

Filter the packet with the “dsn” protocol

<img width="960" height="376" alt="Screenshot 2026-05-01 181922" src="https://github.com/user-attachments/assets/a1242fda-624c-4448-861a-5ed1ac6bd2c6" />

check the packet number after the packet number: 59

On the packet 

```jsx
619	2021-01-31 11:03:12.519257	10.0.2.4	192.168.55.1	DNS	619	83	Standard query 0xcae1 A dunyamuzelerimuzesi.com
```

Answer: dunyamuzelerimuzesi.com

---

8.Decrypt the Tender document and submit the flag.

To decrypt the file “Tender.pdf.micro” we have to install the tool TeslaDecrypt 

I did some digging and found a command line tool that can decrypt files encrypted by the ransomware. You can download it at Mcafee. Instructions on how to use the tool can be found here. Decrypting was relatively easy and opening the document, we get the flag.

```jsx
$ ./tesladecrypt.exe -h
usage: tesladecrypt.exe [-h] [--version] [-l] [-r] [-d] target_directory

positional arguments:
  target_directory  Directory to search for encrypted teslacrypt files

optional arguments:
  -h, --help        show this help message and exit
  --version         Get version information
  -l, --list        List all encrypted TeslaCrypt files
  -r, --recursive   Process files in sub-directories
  -d, --del         Delete encrypted files after decryption
```

```jsx
$ ./tesladecrypt.exe -d E:\
>
Decrypting [ Tender.pdf.micro ] - OK and DELETED Encrypted File
```

<img width="448" height="248" alt="image" src="https://github.com/user-attachments/assets/f88ffb3a-0234-452f-a83b-e600229e4030" />

Answer: `BTLO-T3nd3r-Fl@g`

---
# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1566.001 | Spearphishing Attachment |
| Execution | T1059.005 | Visual Basic |
| Persistence | T1547.001 | Registry Run Keys / Startup Folder |
| Privilege Escalation | - | - |
| Defense Impairment | - | - |
| Credential Access | T1555.003 | Credentials from Web Browsers |
| Discovery | T1083 | File and Directory Discovery |
| Lateral Movement | T1021.001 | Remote Services: SMB/Windows Admin Shares |
| Collection | T1005 | Data from Local System |
| Command and Control | T1071.001 | Application Layer Protocol: Web Protocols |
| Exfiltration | T1041 | Exfiltration Over C2 Channel |
| Impact | T1486 | Data Encrypted for Impact |

## Author

### RUTHRAN-SEC
