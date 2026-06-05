# Web Server Honeypot Compromise Investigation – Log Based Threat Analysis

## CyberDefenders

## Scenario

A web server honeypot was deployed to monitor malicious internet activity. After unusual behavior was detected, logs from the potentially compromised system were collected for investigation.

As a SOC Analyst, you are tasked with analyzing web server logs, authentication logs, and system logs to determine:

- How the attacker gained access
- What actions were performed
- Whether persistence was established
- Indicators of Compromise (IOCs)

The goal is to reconstruct the attack timeline and provide mitigation recommendations.

## Alert

**Alert Name:** Suspicious Activity Detected on Web Honeypot

**Alert Source:** SIEM / Log Monitoring System

**Trigger Indicators:**

- High number of HTTP requests from a single IP
- Multiple failed login attempts
- Suspicious URL requests (e.g., `/wp-admin`, `/phpmyadmin`, `/shell.php`)
- Unusual POST requests
- Unexpected outbound connections

**Severity:** High

## Given Files

```jsx
root@ip-10-48-119-124:~/Downloads/temp_extract_dir/Hammered# ls -alps
total 13488
    4 drwxr-xr-x 5 root root     4096 Jul  3  2010 ./
    4 drwx------ 3 root root     4096 Mar  7 08:49 ../
    4 drwxr-xr-x 2 root root     4096 Jul  3  2010 apache2/
    4 drwxr-xr-x 2 root root     4096 Jul  3  2010 apt/
10088 -rw-r----- 1 root root 10327345 Jul  3  2010 auth.log
  116 -rw-r----- 1 root root   115010 Jul  3  2010 daemon.log
  224 -rw-r----- 1 root root   228019 Jul  3  2010 debug
   36 -rw-r----- 1 root root    35639 May  3  2010 dmesg
   36 -rw-r----- 1 root root    36408 Apr 28  2010 dmesg.0
   96 -rw-r----- 1 root root    96035 Apr 26  2010 dpkg.log
    4 -rw-r--r-- 1 root root      406 Apr 25  2010 fontconfig.log
    4 drwxr-xr-x 2 root root     4096 Mar 16  2010 fsck/
 2428 -rw-r----- 1 root root  2479182 Jul  3  2010 kern.log
   80 -rw-r----- 1 root root    79695 May  3  2010 messages
    0 -rw-r--r-- 1 root root        0 Apr 25  2010 secure
  356 -rw-r--r-- 1 root root   359696 May  3  2010 udev
    4 -rw-r----- 1 root root      213 Mar 18  2010 user.log
```

## Challenge Questions

1.Which service did the attackers use to gain access to the system?
The login details are will be accorded on the auth.log file. Used Linux Command to filter it out and find through which service did the attacker gained access to the system.

```jsx
Command: 
cat auth.log | cut -d ' ' -f5,6,7
```

In that there are:

- Corn jobs
- sshd login

<img width="960" height="391" alt="Screenshot 2026-03-07 154749" src="https://github.com/user-attachments/assets/cd836cf1-33b7-453f-ac71-ca03a90e7769" />
The attacker gained access thourgh the ssh service.

Answer: ssh

---

2.What is the operating system version of the targeted system?

The system related details are stored in message file. 

```jsx
Command:
Head meassgae

```

<img width="949" height="235" alt="Screenshot 2026-03-07 155342" src="https://github.com/user-attachments/assets/b2325b09-b6c5-4491-93c9-6ee534f9afca" />
Answer: 4.2.4-1ubuntu3

---

3.What is the name of the compromised account?
The compromised account was root

<img width="910" height="396" alt="Screenshot 2026-03-07 155807" src="https://github.com/user-attachments/assets/c904f3da-5296-4b73-9f11-6c8e0afb188a" />
Answer: root

---

4.How many attackers, represented by unique IP addresses, were able to successfully access the system after initial failed attempts?

To find out how many attackers where trying gain access on ssh form the different IP, I have tried of filtering the IP and got the results

```jsx
Command:
cat auth.log | grep -i accepted | uniq | sort | awk '{print $9}' | sort | uniq -c 

```

<img width="960" height="243" alt="Screenshot 2026-03-07 180514" src="https://github.com/user-attachments/assets/2a8a3228-556a-4ab5-a68d-b0bc0acbdf44" />
There where totally 6 different users

Answer: 6

---

5.Which attacker's IP address successfully logged into the system the most number of times?
 We know that root was compromised, So i have used root to filter 

```jsx
Command:
cat auth.log | grep -i accepted | grep -i 'root'
```

<img width="930" height="398" alt="Screenshot 2026-03-07 181036" src="https://github.com/user-attachments/assets/c7d1c41e-4b4d-4d5d-8250-879b7738d02b" />
Answer: 219.150.161.20

---

6.How many requests were sent to the Apache Server?
To find out how many requests made we have to move to Apache Server log folder, Where there is www-access.log.

```jsx
Command:
cat www-access.log | wc
```

<img width="960" height="382" alt="Screenshot 2026-03-07 182055" src="https://github.com/user-attachments/assets/9e5a94c4-e3dd-481b-a9aa-a16c40922d16" />
Answer: 365

---

7.How many rules have been added to the firewall?
i don’t know where will the firewall log rules are stored among the given files, So i tried to search all files and folders

```jsx
Command:
grep -r -i iptable *
```

<img width="960" height="350" alt="Screenshot 2026-03-07 183437" src="https://github.com/user-attachments/assets/d7bea7b7-aa7e-4e7a-a9a4-d5f4b13b0da8" />
The firewall rules are changed in iptables 

Answer: 6 

---

8.One of the downloaded files on the target system is a scanning tool. What is the name of the tool?
The downloaded details are recorded in dpkg.log

```jsx
Command:
cat dpkg.log | awk '{print $4}' | sort | uniq
```

<img width="187" height="401" alt="Screenshot 2026-03-07 184552" src="https://github.com/user-attachments/assets/c8575866-d599-4af5-80e8-ac1fd5ca507f" />

Answer: nmap 

---

9.When was the last login from the attacker with IP ? Format: MM/DD/YYYY HH:MM:SS AM

The timestamp details are accorded on the auth.log, Also we know that attacker IP is 219.150.161.20

we have to filter it out with the IP and the last accepted password.

```jsx
Command:
grep -i 'accepted' auth.log | grep -i 219.150.161.20
```

<img width="960" height="318" alt="Screenshot 2026-03-07 185602" src="https://github.com/user-attachments/assets/bd262056-95a7-496b-aa4c-4c56f88aa022" />
Answer: 04/19/2010 05:56:05 AM

---

10.The database showed two warning messages. Please provide the most critical and potentially dangerous one.
I just opened the daemon.log file where i saw the warning, And that was the warning message. we can also use:

```jsx
grep -i 'warning' daemon.log
```

<img width="960" height="388" alt="Screenshot 2026-03-07 190627" src="https://github.com/user-attachments/assets/967c1f04-3961-4468-b6de-b8f4ac87dc52" />
Answer: mysql.user contains 2 root accounts without password!

---

11Multiple accounts were created on the target system. Which account was created on **April 26** at **04:43:15**?
In the question it self they have gave the timestamp so that is very easy to filter in auth.log file using the grep command

```jsx
Command:
grep -i '04:43:15' auth.log 
```

<img width="960" height="227" alt="Screenshot 2026-03-07 191052" src="https://github.com/user-attachments/assets/ea7dd7e9-a7da-4bfe-a195-9fc7881254bd" />
Answer: wind3str0y

---

12.Few attackers were using a proxy to run their scans. What is the corresponding user-agent used by this proxy?
The www.access.log file has the details about the user agent.

```jsx
Command:
cat www-access.log | awk '{print $12}' | uniq | sort
```

<img width="960" height="449" alt="Screenshot 2026-03-07 192430" src="https://github.com/user-attachments/assets/32a0c75e-c38e-47a8-b8d4-bfea0c15aad6" />
Answer: pxyscand/2.1

# MITRE ATT&CK Mapping

| ATT&CK Stage         | Technique ID         | Technique                          |
| -------------------- | -------------------- | ---------------------------------- |
| Reconnaissance       | T1595.001, T1595.003 | Active Scanning, Wordlist Scanning |
| Resource Development | -                    | -                                  |
| Initial Access       | T1110.001, T1078     | Password Guessing, Valid Accounts  |
| Execution            | T1059.004            | Unix Shell                         |
| Persistence          | T1136.001            | Local Account                      |
| Privilege Escalation | -                    | -                                  |
| Defense Impairment   | T1562.004            | Disable or Modify System Firewall  |
| Credential Access    | T1110.001            | Password Guessing                  |
| Discovery            | -                    | -                                  |
| Lateral Movement     | -                    | -                                  |
| Collection           | -                    | -                                  |
| Command and Control  | -                    | -                                  |
| Exfiltration         | -                    | -                                  |
| Impact               | -                    | -                                  |


## Author
### RUTHRAN-SEC
