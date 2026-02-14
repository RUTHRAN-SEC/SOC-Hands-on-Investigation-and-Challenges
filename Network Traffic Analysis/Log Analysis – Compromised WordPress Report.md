## Blue Team Online Lab

## Tools Used: grep, sort, uniq, Apache Log Analyzer

## **Incident Response**

## **Scenario:**

A WordPress website has been compromised. Initial assessment suggests a vulnerable plugin may have allowed remote code execution (RCE), enabling the attacker to gain access to the underlying server operating system. The task is to analyze web server logs to identify malicious activity, exploitation attempts, and attacker behavior.

## Alert

Multiple suspicious HTTP requests detected targeting WordPress plugin directories, including abnormal POST requests and potential command execution patterns in Apache access logs.

## Overall Analysis

After Unzipping the file we got access.log file 

```jsx
head access.log
```

<img width="960" height="349" alt="Screenshot 2026-02-14 125641" src="https://github.com/user-attachments/assets/1549606e-4f57-4035-b71d-e4b452e0ae2b" />


We have got:

- IP v4 address
- Time
- HTTP request
- User Agent

Based on Top talker we have to arrange the IP address, Which has more traffic in network

### IP Address Separation - ip_address.txt

```jsx
cat access.log | cut -d ' ' -f 1 | sort |uniq -c | sort -nr >> ip_address.txt
```

### GET Request - get.txt

```jsx
cat access.log | cut -d '"' -f 2 | sort | grep -i "GET" | uniq | sort >> get.txt
```

### HEAD Request - head.txt

```jsx
cat access.log | cut -d '"' -f 2 | sort | grep -i "HEAD" | uniq | sort >> head.txt
```

### POST Request - post.txt

```jsx
cat access.log | cut -d '"' -f 2 | sort | grep -i "POST" | uniq | sort >> post.txt
```

### User Agent Separation - useragent.txt

```jsx
cat access.log | cut -d '"' -f 6 |cut -d '[' -f 1 |sort | uniq -c | sort -nr >> useragent.txt
```

These .txt files will be useful for the Analyzing while answering the question  

## Challenge Questions

1.Identify the URI of the admin login panel that the attacker gained access to (include the token)

The question have mentioned the token must also be in the answer , So we use a grep command to filter the access.log list

```jsx
cat access.log | grep 'token' | head 
```

<img width="953" height="384" alt="Screenshot 2026-02-14 134908" src="https://github.com/user-attachments/assets/95451245-88bf-426a-8e08-bd9f30ce2ed5" />

Answer:/wp-login.php?itsec-hb-token=adminlogin

--- 

2.Can you find two tools the attacker used?

We have already filtered the user agent, Which is in useragent.txt 

<img width="960" height="374" alt="Screenshot 2026-02-14 135206" src="https://github.com/user-attachments/assets/55340b5b-726b-4168-98bd-072912e7695b" />

Answer: WPScan, sqlmap

--- 

3.The attacker tried to exploit a vulnerability in ‘Contact Form 7’. What CVE was the plugin vulnerable to? (Do some research!)

Just Used Google to search “CVE of Contact form 7”

<img width="960" height="472" alt="Screenshot 2026-02-14 135641" src="https://github.com/user-attachments/assets/7eca9f08-dae1-46e4-bbf0-d45f846a03ae" />

Answer: CVE-2020-35489

--- 

4.What plugin was exploited to get access? 

<img width="561" height="107" alt="Screenshot 2026-02-14 143548" src="https://github.com/user-attachments/assets/f8e69326-dfd1-478e-9046-50f96b336993" />

Searched in google “simple file list plugin version” 

<img width="732" height="400" alt="Screenshot 2026-02-14 143714" src="https://github.com/user-attachments/assets/f9ba9906-3ce0-4407-93d0-0d6ae9b8b930" />

Answer: Simple File list 4.2.2

--- 

5.What is the name of the PHP web shell file?

The exploitation in done by uploading a malicious file in the site, So i have filtered the linux command using grep “upload”. Also the file uploaded request are in post so i have used the file post.txt we have already done.

```jsx
cat post.txt | grep "upload"
```

<img width="541" height="118" alt="Screenshot 2026-02-14 140341" src="https://github.com/user-attachments/assets/4272a037-489f-410a-9801-ccd93209ceed" />

The fr34k.php file seems to be a malicious php code 

Answer: fr34k.php

--- 

6.What was the HTTP response code provided when the web shell was accessed for the final time?

Filtered using the fr34k.php and used tail command to see the last log detail

```jsx
cat access.log | grep 'fr34k.php' | tail
```

<img width="957" height="376" alt="Screenshot 2026-02-14 144639" src="https://github.com/user-attachments/assets/f8de1368-f3d7-4cd5-99b6-592ec9148140" />

Answer: 404
