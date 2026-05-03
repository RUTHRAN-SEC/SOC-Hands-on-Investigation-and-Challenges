# RDP Brute-Force Attack Investigation – Windows Security Log Analysis

## Blue Team Lab Online

## Scenario

A system administrator reported a significant increase in failed authentication attempts on a Windows server. Initial review of the Windows Security Event Logs revealed a large number of Audit Failure events, raising concerns about a potential Remote Desktop Protocol (RDP) brute-force attack.

As part of the investigation, you are tasked with:

- Analyzing Windows Security logs (Event ID 4625, etc.)
- Identifying suspicious login patterns
- Detecting source IP addresses involved in the attack
- Determining targeted user accounts
- Identifying any successful logins following repeated failures
- Assessing the risk of unauthorized access

The objective is to confirm the brute-force attack, evaluate its impact, and recommend mitigation strategies.

## Alert

**Suspicious Authentication Activity Alert:** A high volume of failed login attempts was detected on a Windows system, indicating a potential brute-force attack targeting Remote Desktop services. Security logs show repeated authentication failures across multiple user accounts within a short time frame.

The pattern suggests automated login attempts originating from one or more sources. Immediate investigation is required to identify the attacker, assess whether any accounts were compromised, and implement access control measures.

## Tools Used

- Grep
- Text Editor
- ipinfo

## Given File

```jsx
5892 -rw-r--r-- 1 root root 6032687 Feb 12  2022 BTLO_Bruteforce_Challenge.csv
  68 -rw-r--r-- 1 root root   69632 Feb 12  2022 BTLO_Bruteforce_Challenge.evtx
5892 -rw-r--r-- 1 root root 6032687 Feb 12  2022 BTLO_Bruteforce_Challenge.txt
```

## Challenge Questions

1.How many Audit Failure events are there? (Format: Count of Events) 

As i am using kali linux for the investigation, I will be using the file “BTLO_Bruteforce_Challenge.txt” for the rest of the investigation.

To know the number of failed audit event occurred, We will use the grep command to filter the events and also that i am using the event ID “4625” which indicates a failed logon attempt.

```jsx
cat BTLO_Bruteforce_Challenge.txt | grep "4625" | grep -i "Audit" | wc
```

<img width="702" height="223" alt="Screenshot 2026-05-03 160947" src="https://github.com/user-attachments/assets/38f288b3-7e42-47e3-a8f3-2c6cd09f0e44" />

Answer: 3103

---

2.What is the username of the local account that is being targeted? (Format: Username)

Before knowing who was targeted, We have to know all the usernames that are available.

```jsx
cat BTLO_Bruteforce_Challenge.txt | grep -i "Account name" | sort | uniq -c | sort 
```

<img width="724" height="213" alt="Screenshot 2026-05-03 161626" src="https://github.com/user-attachments/assets/53f1d651-f5bc-44ae-a6d6-df6543820972" />

The account name “administrator” had a more number of counts. So the administrator was the username targeted by the attacker. The attacker made many failed login attempt to gain access to the account.

Answer: administrator

---

3.What is the failure reason related to the Audit Failure logs? (Format: String)

It is a Brute force attempt on the username administrator for trying multiple username and password on the administrator account.

```jsx
cat BTLO_Bruteforce_Challenge.txt | grep -i "Failure reason" | head   
```

<img width="675" height="273" alt="Screenshot 2026-05-03 162142" src="https://github.com/user-attachments/assets/ed372c79-5a55-4306-ba7b-22040a1a4ff4" />

Answer: Unknown user name or bad password.

---

4.What is the Windows Event ID associated with these logon failures? (Format: ID)

We have used Event ID 4625 which indicates a **failed logon attempt**, marking when a user or service fails to authenticate. Common causes include mistyped passwords, expired credentials, locked accounts, or misconfigured services. While single, sporadic occurrences are normal, frequent 4625 events often signify brute force attacks, credential stuffing, or misconfigured network service accounts.

Answer: 4625

---

5.What is the source IP conducting this attack? (Format: X.X.X.X)

```jsx
cat BTLO_Bruteforce_Challenge.txt | head -n 50
```

```jsx
Keywords        Date and Time   Source  Event ID        Task Category
Audit Failure   2/12/2022 7:22:00 AM    Microsoft-Windows-Security-Auditing     4625    Logon   "An account failed to log on.

Subject:
        Security ID:            NULL SID
        Account Name:           -
        Account Domain:         -
        Logon ID:               0x0

Logon Type:                     3

Account For Which Logon Failed:
        Security ID:            NULL SID
        Account Name:           administrator
        Account Domain:

Failure Information:
        Failure Reason:         Unknown user name or bad password.
        Status:                 0xC000006D
        Sub Status:             0xC000006A

Process Information:
        Caller Process ID:      0x0
        Caller Process Name:    -

Network Information:
        Workstation Name:       -
        Source Network Address: 113.161.192.227
        Source Port:            59545

Detailed Authentication Information:
        Logon Process:          NtLmSsp 
        Authentication Package: NTLM
        Transited Services:     -
        Package Name (NTLM only):       -
        Key Length:             0
```

The attacker used Source Network Address: 113.161.192.227

Answer: 113.161.192.227

---

6.What country is this IP address associated with? (Format: Country)

Used the platform IP info: https://ipinfo.io/what-is-my-ip

<img width="960" height="474" alt="Screenshot 2026-05-03 163017" src="https://github.com/user-attachments/assets/0c8a907e-87df-426c-967c-af89ebf06f77" />

Answer: Vietnam

---

7.What is the range of source ports that were used by the attacker to make these login requests?

```jsx
cat BTLO_Bruteforce_Challenge.txt | grep -i "Source Port" | uniq | sort | head
```

```jsx
cat BTLO_Bruteforce_Challenge.txt | grep -i "Source Port" | uniq | sort | tail
```

<img width="955" height="390" alt="Screenshot 2026-05-03 163626" src="https://github.com/user-attachments/assets/209fb081-66c3-4427-89f6-29684566dff6" />

Answer: 49162-65534

---

## Author

### RUTHRAN-SEC
