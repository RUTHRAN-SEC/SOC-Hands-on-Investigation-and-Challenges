# Investigating a Compromised Host

## **Benign CTF**

## TryHackme (Challenge)

### Tools Used: Splunk

### Scenario

One of the client’s IDS indicated a potentially suspicious process execution indicating one of the hosts from the HR department was compromised. Some tools related to network information gathering / scheduled tasks were executed which confirmed the suspicion. Due to limited resources, we could only pull the process execution logs with Event ID: 4688 and ingested them into Splunk with the index **win_eventlogs** for further investigation.

## Alert: Suspicious Process Execution Detected

### About the Network Information

```json
The network is divided into three logical segments. It will help in the investigation.

IT Department:
James
Moin
Katrina

HR department:
Haroon
Chris
Diana

Marketing department:
Bell
Amelia
Deepak
```

### Challenge questions

1.How many logs are ingested from the month of March, 2022?

The Scenario have mentioned that due to fewer source they can pull only **win_eventlogs, So in search field type index=”win_eventlogs” to view the how many events happened

<img width="362" height="130" alt="Screenshot 2026-01-08 223354" src="https://github.com/user-attachments/assets/5c2098f3-69a9-439d-9ba5-9f43c85e060b" />


Answer: 13959

2.Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?

In the username field in clicked the top values to see how many logs where came from the users with their name and count, where this username is suspicious “ Amel1a ” where there was only one count which which made suspicious also the imposter used leetspeak format where “i” replaced by “1”
<img width="959" height="297" alt="Screenshot 2026-01-08 222742" src="https://github.com/user-attachments/assets/6c26fac6-4c45-4a2a-8754-94f1db115cda" />


Answer: Amel1a

3.Which user from the HR department was observed to be running scheduled tasks?

To filter the logs i used [ index=win_eventlogs ProcessName="**schtasks**" ] , Then i viewed the Username field to see the Username and got “ Chris.fort “ with one count , Also We Can filter the search field by [ index=win_eventlogs ProcessName="*sch*" HostName("HR") ]

<img width="421" height="135" alt="Screenshot 2026-01-08 222536" src="https://github.com/user-attachments/assets/751d7df1-bf78-4164-8972-423979dbadd6" />

Answer: Chris.fort

4.Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.

```json
Hint:
Explore lolbas-project.github.io/ to find binaries used to download payloads
```

We know that the imposter is within the HR department so we only filter form that only.

index=win_eventlogs HostName=”*HR*” | top limit=50 CommandLine , In second page we see certutil command that helps to download files in windows.
<img width="960" height="312" alt="Screenshot 2026-01-08 224131" src="https://github.com/user-attachments/assets/d37bec34-c5f8-4913-a97f-056ed615f539" />


click the command view the event button and to see full details about that log 
<img width="591" height="385" alt="Screenshot 2026-01-08 224659" src="https://github.com/user-attachments/assets/89d25b6f-254a-4531-bbe5-e553a5c58ac8" />


Answer: haroon

5.To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?

In the CommandLine field form the previous question we saw that imposter using certutil.exe command to download file

<img width="643" height="389" alt="Screenshot 2026-01-08 224926" src="https://github.com/user-attachments/assets/c50512c7-746a-4273-9b17-1e4085250975" />

Answer: certutil.exe

6.What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)

View the EventTime

<img width="592" height="389" alt="Screenshot 2026-01-08 225159" src="https://github.com/user-attachments/assets/4b1646b1-9ff0-49bd-a395-17c26771bd11" />

Answer:2022-03-04

7.Which third-party site was accessed to download the malicious payload?

View CommandLine

<img width="610" height="397" alt="Screenshot 2026-01-08 225417" src="https://github.com/user-attachments/assets/210d8e48-c3e6-4722-8366-02082b691dad" />


Answer: controlc.com

8.What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?

View CommandLine

<img width="598" height="390" alt="Screenshot 2026-01-08 225608" src="https://github.com/user-attachments/assets/475a1725-4c74-4cd9-8dee-9058418a5e93" />

Answer: benign.exe

9.The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?

To see the malicious content we need to view the URL

<img width="951" height="385" alt="Screenshot 2026-01-08 225928" src="https://github.com/user-attachments/assets/4a7d294e-d26b-4440-b78c-9ffb09404166" />

Answer: THM{KJ&*H^B0}

10.What is the URL that the infected host connected to?

View CommandLine that was the URL infected host connected to

<img width="608" height="383" alt="Screenshot 2026-01-08 225743" src="https://github.com/user-attachments/assets/3bb18ae4-4f25-458e-a944-5ece71ec6327" />

Answer: https://controlc.com/e4d11035

### Done By Ruthran-sec
