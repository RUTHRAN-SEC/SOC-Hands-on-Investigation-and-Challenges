# Slingshot – E-Commerce Web Server Compromise & Log Analysis Investigation

## TRY HACK ME

## Scenario

Slingway Inc., a leading toy manufacturer, identified suspicious activity affecting its e-commerce environment. Security monitoring revealed potential unauthorized actions targeting the public-facing web server, along with indications that the backend database may have been modified without authorization.

To support the investigation, logs from the affected infrastructure were centralized within an Elastic Stack environment and made available through Kibana.

As the assigned SOC Analyst, your objectives are to:

- Analyze web server and application logs
- Identify the attack vector used by the threat actor
- Determine whether unauthorized database modifications occurred
- Reconstruct the attack timeline beginning July 26, 2023
- Identify affected systems, users, and resources
- Assess the overall impact of the compromise

The goal is to determine how the attack occurred, what actions were performed, and whether sensitive business data was affected.

## Alert

**Web Application Security Alert:** Suspicious activity detected on the organization’s e-commerce web server, accompanied by signs of unauthorized database modifications.

Log data indicates abnormal requests targeting web application resources and potential manipulation of backend database records. The activity may have resulted in unauthorized access to business-critical systems and customer-related information.

Immediate log analysis is required to identify the attack method, determine the extent of the compromise, and assess any impact on the organization’s infrastructure.

## Tools Used

- Elastic Search

## Challenge Questions

1.What is the attacker's IP address?

The investigation time line was July 26, 2023 till now. In order to find the attacker IP address we have to find what the attacker done on the Web server. The Alert shows us the attacker is making a modification in the backend. Where it clearly shows that the attacker had gained access to the database of the Web Server.

<img width="203" height="366" alt="Screenshot 2026-06-03 205133" src="https://github.com/user-attachments/assets/117fa8c9-e857-4e05-8f5b-b51524afeebe" />

To start the investigation, We have to see the top talker IP address in the field name **transaction.remote_address.** 

The IP address `10.2.0.15` was the top talker. Investigating on that first.

On the Time: Jul 26, 2023 @ 14:27:08.138

The IP address `10.2.0.15` is using Nmap Script as User Agent to Scan the Web server `10.0.2.4`

So This must be the Attacker IP address.

Answer: `10.2.0.15` 

---

2.What is the first scanner that the attacker ran against the web server?

On the Time line “ Jul 26, 2023 @ 14:27:08.138 ” the attacker first scanning the Web server using the tool Nmap. 

Log message 

```jsx
{"transaction":{"time":"26/Jul/2023:14:27:07 +0000",
"transaction_id":"ZMEtO9IQYNUAdpKzFBecawAAAAQ",
"remote_address":"10.0.2.15",
"remote_port":43416,
"local_address":"10.0.2.4",
"local_port":80},
"request":{"request_line":"GET / HTTP/1.1",
"headers":{"User-Agent":"Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)",
"Connection":"close","Host":"slingway.thm"}},"response":{"protocol":"HTTP/1.1",
"status":200,"headers":{"Vary":"Accept-Encoding","Content-Length":"518",
"Connection":"close","Content-Type":"text/html; charset=UTF-8"}},
"audit_data":{}}
```

<img width="960" height="470" alt="Screenshot 2026-06-04 204414" src="https://github.com/user-attachments/assets/a746dd7f-c923-429c-8e62-f4a2e52f4345" />

The attacker stops the scanning at the Time line of “26 Jul , 2023  14:27:08”

Answer: Nmap Scripting Engine

---

3.What is the User Agent of the directory enumeration tool that the attacker used on the web server?

The attacker used the tool Gobuster tool for enumeration of the Web Server.

The Time line of the Enumeration tool used was “Jul 26, 2023 @ 14:27:43.330”

The User-Agent: `Mozilla/5.0 (Gobuster)`

<img width="960" height="475" alt="Screenshot 2026-06-04 205453" src="https://github.com/user-attachments/assets/7ac3dc82-82a2-40ec-bc45-3ad7715e86f7" />

Answer: Mozilla/5.0 (Gobuster)

---

4.In total, how many `404` responses did the attacker receive when enumerating the web server?

Filter using the IP address of the attacker and the User-Agent Which is “Mozilla/5.0 (Gobuster)”

and finally filter it out by the response code 404.

- transaction.remote_address: `10.0.2.15`
- request.headers.User-Agent: `Mozilla/5.0 (Gobuster)`
- response.status: `404`

<img width="960" height="473" alt="Screenshot 2026-06-04 212512" src="https://github.com/user-attachments/assets/c2cac726-a25c-4b0b-b17d-91248ab50c18" />

The attacker had received totally 1861 response status of 404.

Answer: 1861

---

5.What flag was discovered in one of the directories identified during enumeration?

In order to find the flag we can filter using these:

- transaction.remote_address: `10.0.2.15`
- request.headers.User-Agent: `Mozilla/5.0 (Gobuster)`

And in the Kibana Search bar , Just search for the word “flag”, These is only one log.

<img width="960" height="475" alt="Screenshot 2026-06-04 213716" src="https://github.com/user-attachments/assets/bfc9a1b2-5e8b-48ec-8d2f-ca0508378411" />

Answer: a76637b62ea99acda12f5859313f539a

---

5.What flag was discovered in one of the directories identified during enumeration?

To find the login directory that was discovered by the attacker , We have to filter using these:

- transaction.remote_address: `10.0.2.15`
- request.headers.User-Agent: `Mozilla/5.0 (Gobuster)`

And in the Search bar search for the word “login”

There where totally 3 logs

- /login
- /admin-login.php
- /admin-login

The attacker had discovered the /admin-login.php directory of the admin page.

<img width="960" height="467" alt="Screenshot 2026-06-04 215042" src="https://github.com/user-attachments/assets/02667731-1381-4d63-9419-6b98450e7990" />

Answer: /admin-login.php

---

6.What is the User-Agent of the brute-force tool that the attacker used on the admin panel?

The attacker used the tool hydra for brute forcing the admin login page. The Time line of the attacker using the tool hydra Jul 26, 2023 @ 14:29:01.706

<img width="960" height="468" alt="Screenshot 2026-06-04 215141" src="https://github.com/user-attachments/assets/bc7e897e-28e0-4bae-8195-4b2b99b26967" />

Answer:  Mozilla/4.0 (Hydra)

---

7.What `username:password` combination did the attacker use to gain access to the admin page?

The attacker used the tool hydra and brute forced the admin.login page. Used these filter:

- transaction.remote_address: `10.0.2.15`
- request.headers.User-Agent: `Mozilla/4.0 (Hydra)`
- response.status: `200`

```jsx
{"transaction":{"time":"26/Jul/2023:14:29:04 +0000",
"transaction_id":"ZMEtsNIQYNUAdpKzFBeduwAAAAQ",
"remote_address":"10.0.2.15",
"remote_port":36838,
"local_address":"10.0.2.4",
"local_port":80},"request":{"request_line":"GET /admin-login.php HTTP/1.1",
"headers":{"Host":"slingway.thm",
"Connection":"close",
"Authorization":"Basic YWRtaW46dGh4MTEzOA==",
"User-Agent":"Mozilla/4.0 (Hydra)"}},
"response":{"protocol":"HTTP/1.1",
"status":200,
"headers":{"Content-Length":"1",
"Connection":"close",
"Content-Type":"text/html; charset=UTF-8"}},
"audit_data":{}}
```

The attacker gained the password on the Time of 26 Jul, 2023 14:29:04.

the Authorization field was encoded with the base64 , I have cracked the base64 using the cyber chef tool.

```jsx
YWRtaW46dGh4MTEzOA==
admin:thx1138
```

the attacker gained the username and password to the admin login page.

Answer: admin:thx1138

---

8.What flag was included in the file that the attacker uploaded to the `/admin/upload.php` directory?

We have to see the request that made to the Web server in the directory /admin/upload.php.

The Time Line of uploaded was Jul 26, 2023 @ 14:29:35.820.

The filename="easy-simple-php-webshell.php”

so i have filtered by:

- transaction.remote_address: `10.0.2.15`
- http.url: `/admin/upload.php?action=upload`

In that we have to see the request body of the “request.body”

<img width="960" height="473" alt="Screenshot 2026-06-04 224853" src="https://github.com/user-attachments/assets/dca47c8c-98bd-47dc-8870-9a27bbbbe5e3" />

The request.body contains the flag.

Answer: THM{ecb012e53a58818cbd17a924769ec447

---

9.What was the first command the attacker ran using the web shell?

To find the first command ran by the attacker we have to check the logs after the Web shell uploaded , Also that we know the web shell file name in order to filter the logs that attacker ran command.

In the Search bar search for the file name “easy-simple-php-webshell.php”

- transaction.remote_address: `10.0.2.15`
- response.status: `200`

<img width="959" height="473" alt="Screenshot 2026-06-05 124106" src="https://github.com/user-attachments/assets/d850a60d-6959-4a02-b7dd-bc7256c96516" />

The Time line where the ran first command on Jul 26, 2023 @ 14:29:53.862

The attacker ran the `whoami` command after gain access to the Web shell

Answer: whoami

---

10.Which file was accessed via Local File Inclusion (LFI) to retrieve database credentials?

Local File Inclusion (LFI) is a critical web vulnerability that occurs when a web application takes user input and passes it to a file-inclusion function without proper validation.

On the Time line Jul 26, 2023 @ 14:31:27.332

The attacker used the method **Path Traversal** attack on the Web server to access the file **`config-db.php`**

<img width="958" height="467" alt="Screenshot 2026-06-05 130048" src="https://github.com/user-attachments/assets/c7a341c8-d703-44f7-87c3-ad1ea06bc158" />

Answer: config-db.php

---

11.What is the name of the database the attacker exported via `/phpmyadmin`?

The attacker exported the database on the Time line on Jul 26, 2023 @ 14:33:27.755

Using the filters: 

In the Search bar search for the file name “easy-simple-php-webshell.php”

- transaction.remote_address: `10.0.2.15`
- response.status: `200`

<img width="960" height="473" alt="Screenshot 2026-06-05 131603" src="https://github.com/user-attachments/assets/9a63518e-c7fc-4986-96dc-7f1d90d09fbc" />

The attacker export the database “customer_credit_cards”

Answer: customer_credit_cards

---

12.What flag does the attacker insert into the database using import.php?

On the Time Line Jul 26, 2023 @ 14:34:46.244 

In the Search bar search for the “import.php”

- transaction.remote_address: `10.0.2.15`
- response.status: `200`

<img width="957" height="470" alt="Screenshot 2026-06-05 132211" src="https://github.com/user-attachments/assets/17298c6d-3d21-4354-b181-e18196b061ce" />

Answer:c6aa3215a7d519eeb40a660f3b76e64c

# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | T1595.001, T1595.003 | **Active Scanning: Scanning IP Blocks, Active Scanning: Wordlist Scanning** |
| Resource Development | - | - |
| Initial Access | T1078.002 | **Valid Accounts: Domain Accounts** |
| Execution | T1059 | **Command and Scripting Interpreter** |
| Persistence | T1505.003 | **Server Software Component: Web Shell** |
| Privilege Escalation | T1078.002 | **Valid Accounts: Domain Accounts** |
| Defense Impairment | - | - |
| Credential Access | T1110.001 | **Brute Force: Password Guessing** |
| Discovery | T1083 | **File and Directory Discovery** |
| Lateral Movement | - | - |
| Collection | T1005 | **Data from Local System** |
| Command and Control | T1505.003 | Server Software Component: Web Shell |
| Exfiltration | T1041 | **Exfiltration Over C2 Channel** |
| Impact | T1657 | **Financial Theft** |

### DONE BY

#### RUTHRAN-SEC
