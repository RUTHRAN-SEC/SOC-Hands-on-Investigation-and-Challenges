# Operation Dream Job: Cyber Espionage Campaign Threat Intelligence Report

## HackTheBox

## Scenario

As a Junior Threat Intelligence Analyst at a cybersecurity firm, you have been assigned to investigate a cyber espionage campaign identified as **Operation Dream Job**.

Preliminary intelligence suggests that this campaign targets professionals through deceptive job recruitment lures, with the objective of gaining unauthorized access to corporate environments for espionage purposes.

Your mission is to:

- Identify the threat actor(s) behind the campaign
- Analyze attack vectors and delivery methods
- Document tactics, techniques, and procedures (TTPs)
- Identify malware families used
- Analyze command-and-control (C2) infrastructure
- Determine victimology and targeting patterns
- Map findings to MITRE ATT&CK
- Provide actionable intelligence for defensive teams

## Alert

**Threat Intelligence Tasking: Investigation of Operation Dream Job**

The intelligence team has received reports of an ongoing cyber espionage campaign targeting organizations through fraudulent job recruitment offers.

Initial indicators suggest:

- Spear-phishing emails impersonating recruiters
- Malicious document attachments or links
- Credential harvesting and malware delivery
- Command-and-control infrastructure linked to known threat actors

The objective is to produce an intelligence report detailing the campaign’s scope, infrastructure, attribution, and defensive recommendations.

Severity: **High – Active Espionage Campaign**

## Tool Used

- VirusTotal

## Given Files

```jsx
4 -rwxr-xr-x  1 root root  206 Aug 11  2024 IOCs.txt
```

## File Content

```jsx
1. 7bb93be636b332d0a142ff11aedb5bf0ff56deabba3aa02520c85bd99258406f
2. adce894e3ce69c9822da57196707c7a15acee11319ccc963b84d83c23c3ea802
3. 0160375e19e606d06f672be6e43f70fa70093d2a30031affd2929a5c446d07c1
```

```jsx
1. 7bb93be636b332d0a142ff11aedb5bf0ff56deabba3aa02520c85bd99258406f
```

<img width="837" height="476" alt="Screenshot 2026-03-29 202749" src="https://github.com/user-attachments/assets/b661f91c-f282-4ec6-aeef-b9c6d412fb22" />

```jsx
2.adce894e3ce69c9822da57196707c7a15acee11319ccc963b84d83c23c3ea802
```

<img width="836" height="473" alt="Screenshot 2026-03-29 202856" src="https://github.com/user-attachments/assets/af9e79e8-6452-4245-891b-7ca8047ded61" />

```jsx
3.0160375e19e606d06f672be6e43f70fa70093d2a30031affd2929a5c446d07c1
```

<img width="844" height="476" alt="Screenshot 2026-03-29 202943" src="https://github.com/user-attachments/assets/54153ed7-4bc6-4515-9cf2-a6c2150d6436" />

## Challenge Questions

1.Who conducted Operation Dream Job?

Searched in the google search engine to conduct the OSINT that Who conducted Operation Dream Job.

<img width="960" height="476" alt="Screenshot 2026-04-20 204147" src="https://github.com/user-attachments/assets/625d5fee-56f5-476b-b659-14f597d42a83" />

Refer Link: https://attack.mitre.org/campaigns/C0022/

<img width="960" height="475" alt="Screenshot 2026-04-20 204434" src="https://github.com/user-attachments/assets/b6141806-92f2-4117-b758-048d7795c1c5" />

Operation Dream Job was a cyber espionage operation likely conducted by Lazarus Group

Answer: Lazarus Group

---

2.When was this operation first observed?

On the page right side check out the first seen date

<img width="960" height="474" alt="Screenshot 2026-04-20 205740" src="https://github.com/user-attachments/assets/8df5bed2-4f8a-4dbb-9275-27a869f9f749" />

Answer: September 2019

---

3.There are 2 campaigns associated with Operation Dream Job. One is `Operation North Star`, what is the other?

By scrolling down we can see the title Associated Campaign Descriptions. In that it has mentioned that it was Operation Interception.

<img width="960" height="480" alt="Screenshot 2026-04-20 210035" src="https://github.com/user-attachments/assets/6ad0c982-2d05-4e88-b017-d3555a987045" />

Answer: Operation Interception

---

4.During Operation Dream Job, there were the two system binaries used for proxy execution. One was `Regsvr32`, what was the other?

By scrolling down and take a look at the title “Techniques Used”. Use the search box CTRL+f and search for the word “regsvr32” 

<img width="957" height="474" alt="Screenshot 2026-04-20 210626" src="https://github.com/user-attachments/assets/fb4bbf65-e33f-4579-b083-73733902ba67" />

Mentioned that Rundll32 is a system binary proxy was used During Operation Dream Job, Lazarus Group executed malware.

Answer: Rundll32

---

5.What lateral movement technique did the adversary use?

Near the Techniques Used title there is a ATT&CK Navigator layers click that and view the techniques used by the adversary in the lateral movement.

<img width="960" height="467" alt="Screenshot 2026-04-20 211317" src="https://github.com/user-attachments/assets/80fa4a49-9c60-4361-b26a-6fffe94d2fd7" />

It’s found that adversary have used the Internal Spearphishing technique.

Answer: Internal Spearphishing

---

6.What is the technique ID for the previous answer?

Place the mouse cursor on the internal spearphishing technique. It shows the technique ID.

<img width="935" height="465" alt="Screenshot 2026-04-20 211508" src="https://github.com/user-attachments/assets/1ddc62b9-3208-4bba-8c6b-078a1e6cb158" />

The technique ID of the internal spearphishing was T1534..

Answer: T1534

---

7.What Remote Access Trojan did the Lazarus Group use in Operation Dream Job?

Scroll down you will see the Software Title.

<img width="954" height="473" alt="Screenshot 2026-04-20 212413" src="https://github.com/user-attachments/assets/ff36c897-cdf9-4790-a87d-eb87c489fadf" />

Where the Lazarus Group have used the DRATzarus is a remote access tool (RAT) 

Answer: DRATzarus 

---

8.What technique did the malware use for execution?

Move to the DRATzarus page in MITRE.

Refer Link: https://attack.mitre.org/software/S0694/

<img width="960" height="473" alt="Screenshot 2026-04-20 212807" src="https://github.com/user-attachments/assets/a85b510b-7f14-4bc4-8956-b02daa96e384" />

Click the ATT&CK Navigator Layer and see the execution filed.

<img width="928" height="477" alt="Screenshot 2026-04-20 213013" src="https://github.com/user-attachments/assets/698b8042-2d46-41d2-825b-c8452c79c395" />

The malware has used Natiive API technique for the execution. 

Answer: Native API 

---

9.What technique did the malware use to avoid detection in a sandbox?

Use the Search Bar to find word “SandBox”.

<img width="960" height="473" alt="Screenshot 2026-04-21 115742" src="https://github.com/user-attachments/assets/ffc6bd97-d529-48fb-bd22-498576636d78" />

DRATzarus can use the GetTickCount and GetSystemTimeAsFileTime API calls to measure function timing. DRATzarus can also remotely shut down into sleep mode under specific conditions to evade detection.

Answer: Time Based Checks

---

10.What is the name associated with the first hash provided in the IOC file?

The first hash value in IOC file is:

```jsx
7bb93be636b332d0a142ff11aedb5bf0ff56deabba3aa02520c85bd99258406f
```

<img width="960" height="471" alt="Screenshot 2026-04-21 121320" src="https://github.com/user-attachments/assets/54d723c1-09fe-4983-afc9-5ad35ffed112" />

The name associate with the malware is IEXPLORE.EXE

Answer: IEXPLORE.EXE

---

11.When was the file associated with the second hash in the IOC first created?

The second hash value is:

```jsx
adce894e3ce69c9822da57196707c7a15acee11319ccc963b84d83c23c3ea802
```

<img width="960" height="477" alt="Screenshot 2026-04-21 122222" src="https://github.com/user-attachments/assets/93c513f6-f353-41df-a647-d2640f76c6ca" />

The creation Time of the malware was 2020-05-12 19:26:17

Answer: 2020-05-12 19:26:17

---

12.What is the name of the parent execution file associated with the second hash in the IOC?

In Virustotal the relations field check out the Execution Parents section.

<img width="960" height="476" alt="Screenshot 2026-04-21 123109" src="https://github.com/user-attachments/assets/f85bc12d-430d-4f42-9e4c-05af5a45baac" />

Answer: BAE_HPC_SE.iso

---

13.Examine the third hash provided. What is the file name likely used in the campaign that aligns with the adversary's known tactics?

The third hash value is:

```jsx
0160375e19e606d06f672be6e43f70fa70093d2a30031affd2929a5c446d07c1
```

<img width="960" height="473" alt="Screenshot 2026-04-21 123432" src="https://github.com/user-attachments/assets/0d126fcd-0789-4a26-999a-7386d6b8e6b0" />

The name associate with the hash is Salary_Lockheed_Martin_job_opportunities_confidential.doc 

Answer: Salary_Lockheed_Martin_job_opportunities_confidential.doc

---

14.Which malicious URL in the contacted URLs is used to fetch a secondary .docx file?

<img width="960" height="473" alt="Screenshot 2026-04-21 124026" src="https://github.com/user-attachments/assets/18e152e6-304f-4a18-9f52-7eb03310228f" />

Answer: hxxps[://]markettrendingcenter[.]com/lk_job_oppor[.]docx

---

<img width="482" height="188" alt="Screenshot 2026-04-21 124221" src="https://github.com/user-attachments/assets/df3aa888-4be0-4078-a4ab-69c679fd43a3" />

## Author

### RUTHRAN-SEC
