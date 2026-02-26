# LogJammer - Windows Event Log Investigation

## HackTheBox

## Scenario

As part of a technical assessment for a junior DFIR consultant role at Forela-Security, you are tasked with analyzing Windows Event Logs from a workstation belonging to the user “Cyberjunkie.”

There is suspicion that the user logged into the system and performed potentially malicious actions. Your objective is to:

- Identify login activity
- Detect suspicious logon types
- Investigate privilege escalation attempts
- Analyze process execution events
- Determine if malicious behavior occurred
- Build a timeline of activity

## Alert

**Suspicious User Activity Alert:** Unusual login activity and potentially malicious actions detected from the account “Cyberjunkie” based on Windows Security Event Logs.

Event logs indicate abnormal logon patterns and suspicious process execution that may suggest unauthorized or malicious behavior on the host system.

## Tool Used: Splunk

### Given Files:

- Powershell-Operational.evtx
- Security.evtx
- System.evtx
- Windows Defender-Operational.evtx
- Windows Firewall-Firewall.evtx

## Challenge Questions

1.When did the cyberjunkie user first successfully log into his computer? (UTC)

We can filter out by Event ID “4624”. As we know that event ids are not unique so i have searched in  “Ultimate windows security” site. 

<img width="960" height="514" alt="Screenshot 2026-02-26 115309" src="https://github.com/user-attachments/assets/e49398f1-1d19-4ac8-a8a5-eb1a1c04a7f0" />

The Event ID “4624” Comes under Windows security log Event. For investigation we have also got Security.evtx. And finally i have added the username in last which is cyberjunkie.

```jsx
  Query Used
  index="logjammer" sourcetype="WinEventLog:Security" EventCode=4624 "cyberjunkie"
```

Also make sure to set Time by UTC. By taking a look at first event of successful login which will be bottom in splunk. 

<img width="960" height="471" alt="Screenshot 2026-02-26 120729" src="https://github.com/user-attachments/assets/9b6d2e87-ab0e-4c0e-abd0-f7288f8ef666" />

Answer: 27/03/2023 14:37:09

---

2.The user tampered with firewall settings on the system. Analyze the firewall event logs to find out the Name of the firewall rule added?

We have to analyze the source “Windows Firewall-Firewall.evtx” and added a word “added” in the last.

```jsx
Query Used
index="logjammer" source="C:\\Event-Logs\\Windows Firewall-Firewall.evtx" added
```

<img width="959" height="482" alt="Screenshot 2026-02-26 121942" src="https://github.com/user-attachments/assets/b7985d91-f9d4-4ac2-94d0-7ee6ba7bc509" />

Answer: Metasploit C2 Bypass

---

3.What is the direction of the firewall rule?

The direction of the firewall rule is outbound, so that attacker can perform C2 over the target.

<img width="960" height="469" alt="Screenshot 2026-02-26 122048" src="https://github.com/user-attachments/assets/3b8a77bb-6f5e-4eb2-b98e-e8971b1734ef" />

Answer: Outbound

---

4.The user changed audit policy of the computer. What is the Subcategory of this changed policy?

The audit policy of the windows comes under the source  “Security.evtx” and the Event ID for System audit policy was changed is “4719”

```jsx
Query Used
index="logjammer" source="C:\\Event-Logs\\Security.evtx" EventCode=4719 
```

<img width="960" height="474" alt="Screenshot 2026-02-26 123355" src="https://github.com/user-attachments/assets/643c73eb-5c56-4ab0-aa34-161c63735e25" />

The subcategory of the audit policy was “ Other Object Access Events ”

Answer: Other Object Access Events

---

5.The user "cyberjunkie" created a scheduled task. What is the name of this task?  

The Event ID for a scheduled task was created is “4698” and This comes under the source “Security.evtx”.

```jsx
Query Used 
index="logjammer" source="C:\\Event-Logs\\Security.evtx" EventCode=4698
```

<img width="957" height="469" alt="Screenshot 2026-02-26 124024" src="https://github.com/user-attachments/assets/4f3cd66c-a233-4d8d-8868-96104dce88ec" />

Answer: HTB-AUTOMATION

---

6.What is the full path of the file which was scheduled for the task?

In that same event itself i have added a “c:” in Query to find out the full path of the schedule task

```jsx
Query Used
index="logjammer" source="C:\\Event-Logs\\Security.evtx" EventCode=4698 "c:"
```

<img width="949" height="472" alt="Screenshot 2026-02-26 124558" src="https://github.com/user-attachments/assets/9ff36777-4e53-4788-ac8a-64fe0c3085f3" />

Answer: C:\Users\CyberJunkie\Desktop\Automation-HTB.ps1

---

7.What are the arguments of the command?

Under the Command, There is Argument command.

<img width="960" height="469" alt="Screenshot 2026-02-26 124703" src="https://github.com/user-attachments/assets/60e5f315-d225-430c-a557-4f42d3506a67" />

Answer: -A cyberjunkie@hackthebox.eu

---

8.The antivirus running on the system identified a threat and performed actions on it. Which tool was identified as malware by antivirus?

To know the what malware detected by antivirus, I have searched in google to find the event id 

<img width="726" height="472" alt="Screenshot 2026-02-26 200812" src="https://github.com/user-attachments/assets/8cc86cfd-48b9-41b5-ac62-f863397b1219" />

The Event ID for Malware detected is “1116”

```jsx
Query Used
index="logjammer" source="C:\\Event-Logs\\Windows Defender-Operational.evtx" EventCode=1116
```

<img width="960" height="479" alt="Screenshot 2026-02-26 201057" src="https://github.com/user-attachments/assets/3cf1feb9-319a-4ffc-a416-2be29f2d2f97" />

### Malware - Sharphound

SharpHound is the official C#-based data collector for the BloodHound tool, designed to enumerate Active Directory (AD) environments. It gathers sensitive data including users, groups, and permissions to identify attack paths, which are then analyzed in BloodHound. It is commonly used by both red teams for reconnaissance and security professionals to audit AD security.

Answer: Sharphound

---

9.What is the full path of the malware which raised the alert?

The full path of the malware location was there in the same event where the malware is detected by the antivirus 

<img width="959" height="478" alt="Screenshot 2026-02-26 201453" src="https://github.com/user-attachments/assets/8a2d5fe3-c685-4614-a243-b6298d2d0d51" />

Answer: C:\Users\CyberJunkie\Downloads\SharpHound-v1.1.0.zip

---

10.What action was taken by the antivirus?

Searched is google for event ID for “action taken for antimalware service executable” is “1117”

<img width="739" height="479" alt="Screenshot 2026-02-26 204544" src="https://github.com/user-attachments/assets/d67b2483-4563-48c1-b139-b4c71ed58b59" />

```jsx
Query Used 
index="logjammer" source="C:\\Event-Logs\\Windows Defender-Operational.evtx" EventCode=1117
```

<img width="960" height="473" alt="Screenshot 2026-02-26 204944" src="https://github.com/user-attachments/assets/030ac6ae-573d-4b19-8d67-f816b797b006" />

Answer: Quarantine

---

11.The user used Powershell to execute commands. What command was executed by the user?

The powershell command was executed and the event was recorded on source “Powershell-Operational.evtx. 

<img width="722" height="480" alt="Screenshot 2026-02-26 210356" src="https://github.com/user-attachments/assets/2f3be6da-d504-4afd-abad-e1618ee4e697" />

The event id for the powershell execution is “4104”

```jsx
Query Used
index="logjammer" source="C:\\Event-Logs\\Powershell-Operational.evtx" EventCode=4104
```

<img width="960" height="475" alt="Screenshot 2026-02-26 210525" src="https://github.com/user-attachments/assets/1c34ffe5-2cdd-4e06-bddd-526a69bc5850" />

Answer: Get-FileHash -Algorithm md5 .\Desktop\Automation-HTB.ps1

---

12. We suspect the user deleted some event logs. Which Event log file was cleared? 

The primary Event IDs for clearing Windows event logs are 1102 (Security Log) and 104 (System Log).

I have tried the “1102” with source type “Security.evtx”. I found that the audit logs where cleared.

```jsx
Query Used 
index="logjammer" source="c:\\event-logs\\security.evtx"  EventCode=1102
```

<img width="960" height="469" alt="Screenshot 2026-02-26 122048" src="https://github.com/user-attachments/assets/1c707851-4309-4ebb-bbbc-eaa7bb185e77" />

```jsx
Query Used 
index="logjammer" source="C:\\Event-Logs\\System.evtx" EventCode=104
```

<img width="960" height="473" alt="Screenshot 2026-02-26 215736" src="https://github.com/user-attachments/assets/c791315e-695d-419b-aa5a-390bf5e2c9d7" />

Answer: Microsoft-Windows-Windows Firewall With Advanced Security/Firewall 

---

<img width="590" height="227" alt="Screenshot 2026-02-26 220214" src="https://github.com/user-attachments/assets/667c1ca8-5b0c-4b09-a651-1afcf902941c" />

## Author

### RUTHRAN-SEC
