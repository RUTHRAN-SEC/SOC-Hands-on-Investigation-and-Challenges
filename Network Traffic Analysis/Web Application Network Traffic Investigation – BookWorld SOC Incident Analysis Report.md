# Web Application Network Traffic Investigation – BookWorld SOC Incident Analysis

## CyberDefender

## Scenario

BookWorld, a global online bookstore, detected unusual activity within its production environment following an automated SOC alert. The alert indicated a significant spike in database queries and abnormal server resource consumption during non-peak hours.

Given the organization’s reliance on customer data and online transactions, the anomaly raised immediate concerns regarding potential web application exploitation, unauthorized database access, or data exfiltration attempts.

As the lead SOC analyst, you are tasked with:

- Analyzing captured network traffic
- Identifying the attack vector (e.g., SQL injection, brute force, web exploitation)
- Determining whether customer data was accessed or exfiltrated
- Assessing lateral movement or deeper system compromise
- Recommending containment and mitigation measures

The objective is to preserve system integrity and prevent further exploitation.

## Alert

**Web Application Anomaly Alert:** Automated monitoring systems detected an unusual surge in database queries and elevated server resource utilization within the BookWorld production environment. Network traffic patterns suggest potential malicious activity targeting the web application infrastructure.

Preliminary indicators point toward possible exploitation attempts that may have impacted backend database systems. Immediate network traffic analysis is required to identify the attack vector, evaluate potential data exposure, and determine whether unauthorized access was established within internal systems.

## Tools Used

- **Wireshark**
- **Network Miner**

## Given Files

```jsx
29348 -rw-rw-r-- 1 root root 30050855 Mar 15  2024 WebInvestigation.pcap
```

## Challenge Questions

1.By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?
To find the attacker IP address we have to take a look at the top talker in the Conversation Section in the statistics Section. Got “111.224.250.131” to be suspicious, So i have done a OSINT on it in the IPinfo platform.

<img width="960" height="389" alt="Screenshot 2026-03-23 191324" src="https://github.com/user-attachments/assets/ee3a6746-ff1b-4cbe-99e2-1f2e1c4cdcdd" />
The IP “111.224.250.131” was from Shijiazhuang, Hebei, China. There was more traffic from this IP Address.

<img width="960" height="472" alt="Screenshot 2026-03-23 192227" src="https://github.com/user-attachments/assets/3f51f8ce-ff01-41d9-80c9-757153ee9f88" />
Answer:111.224.250.131

---

2.If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?
In the previous step itself we have performed a OSINT on the IP address, And found out to be from  Shijiazhuang, Hebei, China.

Answer: Shijiazhuang

---

3.Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable PHP script name?
We know that suspicious IP was “ ” So i filtered it out with ip.addr == “ ”

```jsx
Packet detail:
315	1382.007388	111.224.250.131	73.124.22.98	
HTTP 482	GET /search.php?search=test+test HTTP/1.1 
```

On the packet number 315 the attacker made a GET request.

I confirmed this by looking at these packet also

```jsx
347 1450.030622 111.224.250.131 73.124.22.98 HTTP 433 GET /search.php?search=book%27 HTTP/1.1

357	1470.702710	111.224.250.131	73.124.22.98	HTTP	452	GET /search.php?search=book%20and%201=1;%20--%20- HTTP/1.1 
```

On the packet 347 the attacker used “%27” which is URL encoded single quote ( ' ). Where the attacker is trying to do SQL injection. 

On the packet 357 the attacker used “book%20and%201=1;%20--%20-”. When decode it shows book and 1=1; -- - . Which is clear sign of SQL injection on the URL path.

Answer: search.php

---

4.Establishing the timeline of an attack, starting from the initial exploitation attempt, what is the complete request URI of the first SQLi attempt by the attacker?

In the previous question the attacker performed the SQL injection on the URL.

```jsx
357	1470.702710	111.224.250.131	73.124.22.98	HTTP	452	GET /search.php?search=book%20and%201=1;%20--%20- HTTP/1.1 
```

Where this was not the first injection, this was the second one.

Answer: /search.php?search=book and 1=1; -- -

---

5.Can you provide the complete request URI that was used to read the web server's available databases?

The SQL injection was done by the tool SQLmap.

```jsx
1520	1757.931477	111.224.250.131	73.124.22.98	HTTP	457	GET /search.php?search=book%27%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x7178766271%2CJSON_ARRAYAGG%28CONCAT_WS%280x7a76676a636b%2Cschema_name%29%29%2C0x7176706a71%29%20FROM%20INFORMATION_SCHEMA.SCHEMATA--%20- HTTP/1.1 
```

On this packet the sql injection was used to read the server databases.

Answer: /search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,schema_name)),0x7176706a71) FROM INFORMATION_SCHEMA.SCHEMATA-- -

---

6.Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?

```jsx
1681	1978.394217	111.224.250.131	73.124.22.98	HTTP	159	GET / HTTP/1.1 
```

From this packet the attacker is doing brute force for finding directories using the tool called gobuster.

The table name is customer where it contains the details of the user

Answer: customers

---

7.The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide the name of the directory discovered by the attacker?
The attacker is using tool called gobuster where is it doing directories enumeration.

```jsx
88652	2016.511839	111.224.250.131	73.124.22.98	HTTP	414	GET /admin/ HTTP/1.1 
```

The attacker found the directory /admin/.

Answer: /admin/

---

8.Knowing which credentials were used allows us to determine the extent of account compromise. What are the credentials used by the attacker for logging in?

```jsx
88654	2016.519013	111.224.250.131	73.124.22.98	HTTP	469	GET /admin/login.php HTTP/1.1 
```

On the packet number 88654, Where the attacker manually visited the admin login page.

```jsx
88699	2294.305526	111.224.250.131	73.124.22.98	HTTP	661	POST /admin/login.php HTTP/1.1  (application/x-www-form-urlencoded)
```

The attacker tried the password manually also that the password was too weak to be guess.

<img width="639" height="393" alt="Screenshot 2026-03-23 210256" src="https://github.com/user-attachments/assets/e7edb4e5-0223-4072-9f70-e166f3796f84" />

Answer: admin:admin123!

---

9.We need to determine if the attacker gained further access or control of our web server. What's the name of the malicious script uploaded by the attacker?
The attacker must be uploaded the .php through POST request. So I filtered it out in wireshark search bar. 

Filter: 

```jsx
 http.request.method == "POST"
```

```jsx
88757	2697.157173	111.224.250.131	73.124.22.98	HTTP	1122	POST /admin/index.php HTTP/1.1  (application/x-php)
```

<img width="649" height="398" alt="Screenshot 2026-03-23 210932" src="https://github.com/user-attachments/assets/fd82626e-b5a9-4e66-bf5b-e94b98f23b43" />

Answer: NVri2vhp.php

## Author

### RUTHRAN-SEC
