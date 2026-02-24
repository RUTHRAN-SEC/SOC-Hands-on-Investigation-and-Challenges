# Bumblebee - Credential Theft Investigation

## HackTheBox

## Scenario

An external contractor accessed Forela’s internal forum using the Guest Wi-Fi network.

Shortly after access, suspicious activity was detected involving the administrative account.

The security team suspects that the contractor may have stolen administrator credentials.

Logs from the forum application and a full SQLite3 database dump have been provided for investigation.

The objective is to determine:

- How the credentials were obtained
- Whether unauthorized login occurred
- What actions were performed using the admin account
- If sensitive data was accessed or modified


## Alert

**Unauthorized Access Alert:** Suspicious administrative account activity detected following access from Guest Wi-Fi network.

Application logs indicate potential credential compromise and unauthorized login attempts from an external contractor session, suggesting possible privilege escalation and data exposure.

---

## Tools Used: DB Browser for sqlite

## Overview Analysis

There where two files

- access.log
- phpbb.sqlite3

Collecting the important log information from the access.log.

- IP → ip.txt
- GET and POST → get.txt and post.txt
- User agent → useragent.txt

---

## Challenge Questions

1.What was the username of the external contractor?

While Analyzing there is no username details in access.log file. Opened phpbb.sqlite3 on db browser for sqlite tool used for accessing and read the files like .sqlite and .db

On the phpbb_config table, Found a new user name apoole1 added.

<img width="960" height="540" alt="Screenshot 2026-02-24 112334" src="https://github.com/user-attachments/assets/68599963-1bf1-45f4-b8f9-13dcb44aaeb0" />

Answer: apoole1

---

2.What IP address did the contractor use to create their account?

On the same table phpbb_config scrolling down, we can see that the username can be created using that server IP address. Also that there is no other IP address mentioned in that table.

<img width="960" height="540" alt="Screenshot 2026-02-24 114310" src="https://github.com/user-attachments/assets/e509384f-1db1-4411-9d71-5e5bb1f979b4" />

Answer: 10.10.0.76

---

3.What is the post_id of the malicious post that the contractor made?

The post_id was in the phpbb_posts table. We know that 10.10.0.76 is that malicious IP, so we can confirm the post ID 

<img width="959" height="540" alt="Screenshot 2026-02-24 120959" src="https://github.com/user-attachments/assets/09117494-a339-464e-9bcc-921123c4cff1" />

Answer: 9

---

4.What is the full URI that the credential stealer sends its data to?

On the previous phpbb_posts table, scrolling right side we can see post_txt column where the 3rd row where the IP 10.10.0.76 is located, So the post text contains the URI.  

<img width="960" height="540" alt="Screenshot 2026-02-24 122039" src="https://github.com/user-attachments/assets/d564dc2a-bf6e-45b9-833c-688425fb4024" />

Copied that full post txt and pasted in cyberchef tool to extract the url

<img width="959" height="472" alt="Screenshot 2026-02-24 122805" src="https://github.com/user-attachments/assets/3ecfa1c5-ffc1-4454-8b61-66485db21b73" />

We know that IP 10.10.0.76 is the malicious one, also the exploitation is happened by uploading the vulnerable .php code.

Answer: hxxp[://]10[.]10[.]0[.]78/update[.]php

---

5.When did the contractor log into the forum as the administrator? (UTC) 

<img width="960" height="540" alt="Screenshot 2026-02-24 123453" src="https://github.com/user-attachments/assets/094bb838-fcd7-4471-8c93-11052aa2d359" />

On the phpbb_log table contains the information about the user login. On the log_time column contains time in epoch. So i have used a platform epoch converter and pasted the epoch number 

<img width="645" height="400" alt="Screenshot 2026-02-24 123707" src="https://github.com/user-attachments/assets/da904b8e-df6f-4fa0-bb7e-84727ab8cbd2" />

Answer: 26/04/2023 10:53:12 

---

6.In the forum there are plaintext credentials for the LDAP connection, what is the password?

On the phpbb_config there is password in plaintext for LDAP connection 

<img width="960" height="540" alt="Screenshot 2026-02-24 124357" src="https://github.com/user-attachments/assets/a7338e72-394a-4483-a87c-2f4f392903a4" />

Answer: Passw0rd1

---

7.What is the user agent of the Administrator user?

The command i have used to filter out the admin and have a look at request and User agent at same time.

```jsx
cat access.log | grep -i 'admin'| cut -d '"' -f 2,6 | head
```

<img width="960" height="344" alt="Screenshot 2026-02-24 125103" src="https://github.com/user-attachments/assets/e9197d8e-0b38-4d06-b485-c663ce7018ca" />

Answer: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36

---

8.What time did the contractor add themselves to the Administrator group? (UTC)

On the log_time column contains time in epoch. So i have used a platform epoch converter and pasted the epoch number

<img width="960" height="540" alt="Screenshot 2026-02-24 130018" src="https://github.com/user-attachments/assets/4a5ccb3c-c880-421b-b98b-37b3413e3891" />

Answer: 26/04/2023 10:53:51

---

9.What time did the contractor download the database backup? (UTC)

On the access.log, I have used command to filter out and find the back log 

```jsx
grep -i 'GET' access.log | cut -d '"' -f 1,2|grep -i 'backup'
```

<img width="960" height="414" alt="Screenshot 2026-02-24 201633" src="https://github.com/user-attachments/assets/abcaebea-0e6a-44dd-8983-ac7ac7a03122" />

Answer: 26/04/2023 11:01:38

---

10.What was the size in bytes of the database backup as stated by access.log?

I have used that same command but added one field extra to see the byte size of the backup

```jsx
grep -i 'GET' access.log | cut -d '"' -f 2,3|grep -i 'backup'
```

<img width="960" height="268" alt="Screenshot 2026-02-24 202915" src="https://github.com/user-attachments/assets/7aae61a5-1d3d-44c1-a167-27890f89e948" />

Answer: 34707

---

<img width="597" height="228" alt="Screenshot 2026-02-24 203127" src="https://github.com/user-attachments/assets/c5592510-30d4-4175-8546-28fffa65c610" />

## Author

### RUTHRAN-SEC
