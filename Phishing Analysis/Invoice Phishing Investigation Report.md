## **HawkEye Lab**

## Cyber Defenders

### Tools Used: **Wireshark, Brim, Apackets, MaxMind Geo IP, VirusTotal**

## Network Forensics

### Scenario:

An accountant within the organization received an email claiming to contain an **invoice**, which included a **download link**. After opening the email, **suspicious network activity** was detected from the user’s workstation.

As a **SOC Analyst**, your task is to investigate the provided **network trace** to identify any signs of malicious communication, determine whether **data exfiltration** occurred, and assess the potential impact of the incident.

## **Alert**

Suspected Data Exfiltration Following Phishing Email

**Severity:** High

### **Alert Summary:**

Suspicious outbound network traffic was detected from an accountant’s workstation shortly after an invoice-themed phishing email was opened. The observed activity suggests a possible **malware infection** attempting to **exfiltrate data** to an external destination.

## Overall Analysis

We will Analysis based on the top talker with high bytes size and the internal ip is talking to the external ip 

<img width="955" height="329" alt="Screenshot 2026-02-07 133744" src="https://github.com/user-attachments/assets/7562db81-daa1-460d-b30d-576cbb864bec" />

Filtered the field with “ ip.addr == 217.182.138.150 “

```json
In Packet 210 on 2019-04-10 20:37:54.727276
10.4.10.132	217.182.138.150	HTTP	392	GET /proforma/tkraw_Protected99.exe HTTP/1.1 
Downloaded file “tkraw_Protected99.exe“
```

I preformed OSINT on 217.182.138.150 it has Community Score has 2/93 also marked as malicious 

Filtered out with “ DNS “

```json
In packet 204	on 2019-04-10 20:37:53.791017	
10.4.10.132	10.4.10.4	DNS	81	Standard query 0xa002 A proforma-invoices.com

this was before packet 210 so this must be the suspicious email domain
"proforma-invoices.com"

In packet 3159 on 2019-04-10 20:38:15.672284	
10.4.10.132	10.4.10.4	DNS	85	Standard query 0x3f59 A bot.whatismyipaddress.com

whatismyipaddress was schedule after downloading the thraw_Protected99.exe file
```

Performed OSINT on proforma-invoices.com where Community Score 10/93. Marked as Phishing and malicious also had relation with the ip 217.182.138.150.

We know that it was a phishing email so we filter it out with “SMTP” 

```json
In packet 3175	2019-04-10 20:38:16.289945	
23.229.162.69	10.4.10.132	SMTP	251	S: 220-p3plcpnl0413.prod.phx3.secureserver.net ESMTP Exim 4.91 #1 Wed, 10 Apr 2019 13:38:15 -0700  
| We do not authorize the use of this system to transport unsolicited,  
| and/or bulk e-mail.
```

Follow → TCP stream 

```json
The User PC -> EHLO Beijing-5cd1-PC
The login email -> sales.del@macwinlogistics.in
The password -> Sales@23

The subject -> HawkEye Keylogger - Reborn v9 - Passwords Logs - roman.mcguire 
\ BEIJING-5CD1-PC - 173.66.146.112

The Content:
HawkEye Keylogger - Reborn v9
Passwords Logs
roman.mcguire \ BEIJING-5CD1-PC

==================================================
URL               : https://login.aol.com/account/challenge/password
Web Browser       : Internet Explorer 7.0 - 9.0
User Name         : roman.mcguire914@aol.com
Password          : P@ssw0rd$
Password Strength : Very Strong
User Name Field   : 
Password Field    : 
Created Time      : 
Modified Time     : 
Filename          : 
==================================================

==================================================
URL               : https://www.bankofamerica.com/
Web Browser       : Chrome
User Name         : roman.mcguire
Password          : P@ssw0rd$
Password Strength : Very Strong
User Name Field   : onlineId1
Password Field    : passcode1
Created Time      : 4/10/2019 2:35:17 AM
Modified Time     : 
Filename          : C:\Users\roman.mcguire\AppData\Local\Google\Chrome\User Data\Default\Login Data
==================================================

==================================================
Name              : Roman McGuire
Application       : MS Outlook 2002/2003/2007/2010
Email             : roman.mcguire@pizzajukebox.com
Server            : pop.pizzajukebox.com
Server Port       : 995
Secured           : No
Type              : POP3
User              : roman.mcguire
Password          : P@ssw0rd$
Profile           : Outlook
Password Strength : Very Strong
SMTP Server       : smtp.pizzajukebox.com
SMTP Server Port  : 587
==================================================
```

Now we can perform OSINT on the tkraw_Protected99.exe file 

File → export → http

<img width="560" height="409" alt="Screenshot 2026-02-07 143211" src="https://github.com/user-attachments/assets/320c1099-bfa5-4f94-a613-4d58d095118b" />

The first step once we downloaded the file, create a hash value And perform OSINT on the VirusTotal

SHA256: 62099532750dad1054b127689680c38590033fa0bdfa4fb40c7b4dcb2607fb11

<img width="940" height="368" alt="Screenshot 2026-02-07 143919" src="https://github.com/user-attachments/assets/88e73a2e-0e2f-471e-b9e6-78a0c31a7fdc" />

### Challenge Questions

1.How many packets does the capture have?

Open statistics → Capture file properties  
<img width="634" height="401" alt="Screenshot 2026-02-06 130419" src="https://github.com/user-attachments/assets/ae579436-fe3a-437f-8e7a-8c0c1f3c90ce" />

Answer: 4003

2.At what time was the first packet captured (UTC)?

Open statistics → Capture file properties  
<img width="632" height="402" alt="Screenshot 2026-02-06 130852" src="https://github.com/user-attachments/assets/d3361238-fd3b-46f1-9003-eeaee6e4beea" />

Answer: 2019-04-10 20:37


3.What is the duration of the capture?

Open statistics → Capture file properties  

<img width="473" height="403" alt="Screenshot 2026-02-06 194322" src="https://github.com/user-attachments/assets/83c973ee-6092-482d-94ea-7db88eabd686" />

Answer: 01:03:41

4.What is the most active computer at the link level?
Open statistics → Conversations. In the question they have asked computer in the link level so it must be a MAC Address.

<img width="958" height="297" alt="Screenshot 2026-02-06 195019" src="https://github.com/user-attachments/assets/6ecab83f-ee4d-4f21-8ecb-e622f9845333" />

Answer: 00:08:02:1c:47:ae

5.Manufacturer of the NIC of the most active system at the link level?
Open statistics → Conversations. In that we need only the first three character to find out who is the actual manufacturer  

<img width="959" height="319" alt="Screenshot 2026-02-06 195633" src="https://github.com/user-attachments/assets/28a820e1-25b2-46a0-815e-e626cc83b996" />

The Mac address details 

<img width="540" height="406" alt="image" src="https://github.com/user-attachments/assets/519a73cb-ada4-442d-92f6-0c6f4830d3d1" />

In Google search for the Wireshark OUI,  Click on the first link “OUI Lookup Tool” 

<img width="748" height="230" alt="Screenshot 2026-02-06 200059" src="https://github.com/user-attachments/assets/5878b932-5bce-44d0-bb2e-d57b5b102516" />

Paste the three character copied in mac address “ 00:08:02 ” from “ 00:08:02:1c:47:ae “.

<img width="795" height="327" alt="Screenshot 2026-02-06 200346" src="https://github.com/user-attachments/assets/acfb93bf-0089-4de3-b99e-a9f65388d841" />

Answer: Hewlett-Packard

6.Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?

Search in the google for the location of Hewlett Packard company 

<img width="552" height="357" alt="Screenshot 2026-02-06 200622" src="https://github.com/user-attachments/assets/08753168-103d-43be-a251-8bf5def64760" />

Answer: Palo Alto

7.The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?

Open Statistics → Endpoints. In that there are 4 private ip address. But the ip that ends with 255 is a board cast address so we does not count that. The Private IP address ranges, reserved by IANA for internal networking, are 10.0.0.0–10.255.255.255 (Class A), 172.16.0.0–172.31.255.255 (Class B), and 192.168.0.0–192.168.255.255 (Class C). 

<img width="960" height="299" alt="Screenshot 2026-02-06 201023" src="https://github.com/user-attachments/assets/e5ab11d4-fe43-4a86-b1d8-abd6987bac8d" />

Answer: 3

8.What is the name of the most active computer at the network level?

We found that when filtering it out with the SMTP on packet 3175, the pc name was “Beijing-5cd1-PC”

<img width="640" height="421" alt="Screenshot 2026-02-07 144333" src="https://github.com/user-attachments/assets/15345e42-89a5-44aa-b999-3898fa34fc41" />

Answer: Beijing-5cd1-PC

9.What is the IP of the organization's DNS server?

We filter it out with “DNS”

```json
In packet 204	on 2019-04-10 20:37:53.791017	
10.4.10.132	10.4.10.4	DNS	81	Standard query 0xa002 A proforma-invoices.com
```

Answer: 10.4.10.4	

10.What domain is the victim asking about in packet 204?

```json
In packet 204	on 2019-04-10 20:37:53.791017	
10.4.10.132	10.4.10.4	DNS	81	Standard query 0xa002 A proforma-invoices.com
```

Answer: proforma-invoices.com

11.What is the IP of the domain in the previous question?

We have to perform proforma-invoice.com OSINT in VirusTotal. We see that 217.182.138.150 ip address of that domain 

<img width="958" height="476" alt="Screenshot 2026-02-07 144855" src="https://github.com/user-attachments/assets/dabf9bab-ee80-4e72-9e12-7e703e7ab927" />

Answer: 217.182.138.150 

12.Indicate the country to which the IP in the previous section belongs.
Perform OSINT on the ip 217.182.138.150 

<img width="944" height="311" alt="Screenshot 2026-02-07 145643" src="https://github.com/user-attachments/assets/f2b5a586-fed5-475f-aca1-06fbb9096afb" />

Answer: France

13.What operating system does the victim's computer run?

```json
In Packet 210 on 2019-04-10 20:37:54.727276
10.4.10.132	217.182.138.150	HTTP	392	GET /proforma/tkraw_Protected99.exe HTTP/1.1 
```

<img width="634" height="428" alt="Screenshot 2026-02-07 153941" src="https://github.com/user-attachments/assets/cc9ac721-7567-49ea-a72a-d6f199581a54" />

Answer: Windows NT 6.1

14.What is the name of the malicious file downloaded by the accountant?
Filtered the field with “ ip.addr == 217.182.138.150 “

```json
In Packet 210 on 2019-04-10 20:37:54.727276
10.4.10.132	217.182.138.150	HTTP	392	GET /proforma/tkraw_Protected99.exe HTTP/1.1 
Downloaded file “tkraw_Protected99.exe“
```

Answer: tkraw_Protected99.exe

15.What is the md5 hash of the downloaded file?

<img width="960" height="245" alt="Screenshot 2026-02-07 154549" src="https://github.com/user-attachments/assets/05a1ecae-0af8-41bd-b1cd-a88d24b7edd0" />

Answer: 71826ba081e303866ce2a2534491a2f7

16.What software runs the webserver that hosts the malware?

<img width="640" height="424" alt="Screenshot 2026-02-07 164615" src="https://github.com/user-attachments/assets/edd95ef5-160c-4614-af84-9d8784ad3a98" />

Answer: LiteSpeed

17.What is the public IP of the victim's computer?

In the email subject we can see the public ip address of the victim’s computer 

```json
The subject -> HawkEye Keylogger - Reborn v9 - Passwords Logs - roman.mcguire
 \ BEIJING-5CD1-PC - 173.66.146.112
```

Answer: 173.66.146.112

18.In which country is the email server to which the stolen information is sent?

Using the public ip we found in email subject “173.66.146.112”. We need to perform OSINT on it using the Whois platform to find the country detail 

<img width="582" height="320" alt="Screenshot 2026-02-08 114333" src="https://github.com/user-attachments/assets/3da10fe0-bdc3-46ba-95bc-22b4265b9aff" />

Answer: United States 

19.Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?

Filter out with the smtp traffic and check the first packet 

```json
In packet 3175 on 2019/100 20:38:16.289945	23.229.162.69	10.4.10.132	SMTP	
251	S: 220-p3plcpnl0413.prod.phx3.secureserver.net ESMTP Exim 4.91 #1 Wed, 
10 Apr 2019 13:38:15 -0700  
| We do not authorize the use of this system to transport unsolicited,  
| and/or bulk e-mail.
```

<img width="639" height="425" alt="Screenshot 2026-02-08 120509" src="https://github.com/user-attachments/assets/33fefc9b-eb61-49d0-b0ac-15a4409e04b6" />

Answer: Exim 4.91

20.To which email account is the stolen information sent?

Inspect the packet to view the email which is encoded in basse64 

```json
In packet 3175	2019-04-10 20:38:16.289945	
23.229.162.69	10.4.10.132	SMTP	251	S: 220-p3plcpnl0413.prod.phx3.secureserver.net ESMTP Exim 4.91 #1 Wed, 10 Apr 2019 13:38:15 -0700  
| We do not authorize the use of this system to transport unsolicited,  
| and/or bulk e-mail.
```

It was encoded in base64 we decode it with the cyberchef and we got the email that was sent 

Answer: sales.del@macwinlogistics.in

21.What is the password used by the malware to send the email?

There is password encoded under the email login 

```json
In packet 3175	2019-04-10 20:38:16.289945	
23.229.162.69	10.4.10.132	SMTP	251	S: 220-p3plcpnl0413.prod.phx3.secureserver.net ESMTP Exim 4.91 #1 Wed, 10 Apr 2019 13:38:15 -0700  
| We do not authorize the use of this system to transport unsolicited,  
| and/or bulk e-mail.
```

Answer: Sales@23

22.Which malware variant exfiltrated the data?
In the email subject we can see the variant of the malware 

```json
The subject -> HawkEye Keylogger - Reborn v9 - Passwords Logs - roman.mcguire 
\ BEIJING-5CD1-PC - 173.66.146.112
```

Search in google about the malware 

<img width="594" height="403" alt="Screenshot 2026-02-08 122346" src="https://github.com/user-attachments/assets/09eb050c-38b7-41b0-a302-aa3d24d9dfcb" />

Answer: Reborn v9

23What are the bankofamerica access credentials? (username:password)

This was is the email content which is encoded in base64 

```json
In packet 3202 on 2019/100 20:38:16.712598	
10.4.10.132	23.229.162.69	SMTP	56	C: DATA fragment, 2 bytes

==================================================
URL               : https://www.bankofamerica.com/
Web Browser       : Chrome
User Name         : roman.mcguire
Password          : P@ssw0rd$
Password Strength : Very Strong
User Name Field   : onlineId1
Password Field    : passcode1
Created Time      : 4/10/2019 2:35:17 AM
Modified Time     : 
Filename          : C:\Users\roman.mcguire\AppData\Local\Google\Chrome\User Data\Default\Login Data
==================================================

```

Answer: roman.mcguire:P@ssw0rd$

24.Every how many minutes does the collected data get exfiltrated?
filter it out with the smtp. The exfiltration happened on 20:38 

<img width="954" height="286" alt="Screenshot 2026-02-08 123251" src="https://github.com/user-attachments/assets/60544173-64d3-404b-bed4-631f9293b157" />

And the next exfiltration happened on 20:48

<img width="955" height="269" alt="Screenshot 2026-02-08 123447" src="https://github.com/user-attachments/assets/47a85eaf-ea63-4ca2-bb43-1e6f06ea3c40" />

So that is approximately 10 minutes of exfiltration happened 

Answer: 10

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | T1587.001 | Develop Capabilities: Malware |
| Initial Access | T1566.001 | Phishing: Spearphishing Attachment |
| Execution | T1204.002 | User Execution: Malicious File |
| Persistence | - | - |
| Privilege Escalation | - | - |
| Defense Evasion | - | - |
| Credential Access | - | - |
| Discovery | - | - |
| Lateral Movement | - | - |
| Collection | - | - |
| Command and Control | - | - |
| Exfiltration | - | - |
| Impact | - | - |

## DONE BY

### RUTHRAN-SEC
