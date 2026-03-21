# PhishStrike Investigation – Targeted Phishing Email & Malicious Link Analysis

## CyberDefender

## Scenario

A cybersecurity analyst at an educational institution received a security alert regarding a suspicious email targeting faculty members. The email impersonated a trusted contact and referenced a fraudulent $625,000 purchase, urging recipients to download an attached invoice via an embedded link.

Given the financial theme and impersonation tactic, the email was suspected to be part of a phishing campaign aimed at credential harvesting or malware delivery.

As part of the investigation, your objectives are to:

- Analyze the email headers for spoofing indicators
- Examine the embedded link for malicious redirection
- Identify associated domains, IP addresses, and infrastructure
- Extract and validate Indicators of Compromise (IOCs)
- Document findings and recommend awareness measures

The goal is to prevent financial fraud and enhance phishing detection awareness among faculty members.

## Alert

**Phishing Email Targeting Faculty Alert:** A suspicious email impersonating a trusted contact was delivered to multiple faculty members, claiming a high-value purchase and urging recipients to download an invoice via an embedded link. Header analysis and threat intelligence correlation suggest potential spoofing and malicious redirection.

The email may be part of a broader phishing campaign aimed at credential harvesting or malware delivery. Immediate investigation is required to identify malicious infrastructure, extract IOCs, and prevent user interaction with the fraudulent content.

## Tools Used

- Email Header Analyzer
- URLHaus
- URLScan
- VirusTotal
- MalwareBazaar
- VMRay

## Given Files

```jsx
20 -rw-rw-r-- 1 root root 18301 Jul 22  2024 194-PhishStrike.eml
```

## Challenge Questions

1.Identifying the sender's IP address with specific SPF and DKIM values helps trace the source of the phishing email. What is the sender's IP address that has an SPF value of softfail and a DKIM value of fail?
Open the email in the text editor in order to analysis the file the sender IP is mention with the SPF and DKIM where failed 

```jsx
ARC-Authentication-Results: i=2; mx.microsoft.com 1; spf=softfail (sender ip
 is 18.208.22.104) smtp.rcpttodomain=fsfb.org.co smtp.mailfrom=uptc.edu.co;
 dmarc=none action=none header.from=uptc.edu.co; dkim=fail (no key for
 signature) header.d=uptc.edu.co; arc=fail (35)
```

<img width="960" height="364" alt="Screenshot 2026-03-20 220442" src="https://github.com/user-attachments/assets/9fa93bac-b69c-4869-8d55-197a43d97fea" />
Answer: 18.208.22.104

---

2.Understanding the return path of an email is essential for tracing its origin. What is the return path specified in this email?
A return-path or envelope from is a hidden email header that specifies where bounce messages and delivery failure notifications are sent, distinct from the visible "From" address. The return path will be mentioned in return-path field.

```jsx
Return-Path: erikajohana.lopez@uptc.edu.co
```

<img width="943" height="370" alt="Screenshot 2026-03-20 220654" src="https://github.com/user-attachments/assets/037db1f1-3280-4cf9-8ffb-ae5765e2ac97" />
Answer: erikajohana.lopez@uptc.edu.co

---

3.Identifying the source of malware is critical for effective threat mitigation and response. What is the IP address of the server hosting the malicious file related to malware distribution?
We need to find the IP address of the malicious file that’s been hosted, Found the IP that was in the Invoice document.

<img width="960" height="351" alt="Screenshot 2026-03-20 221430" src="https://github.com/user-attachments/assets/574c7712-27c2-4515-b9e9-42825c95100f" />
where it contains file “install.exe” which would me a malware. So done a OSINT on the suspicious IP address. “107.175.247.199”

<img width="960" height="476" alt="Screenshot 2026-03-20 221749" src="https://github.com/user-attachments/assets/b1d0c581-9787-47d9-a2b0-325fa5423734" />
Answer: 107.175.247.199

---

4.Identifying malware that exploits system resources for cryptocurrency mining is critical for prioritizing threat mitigation efforts. The malicious URL can deliver several malware types. Which malware family is responsible for cryptocurrency mining?
We have to do OSINT on the IP and URL cause there is less info about the IP and URL in VirusTotal

<img width="958" height="477" alt="Screenshot 2026-03-20 223854" src="https://github.com/user-attachments/assets/9f49d9de-03e5-4133-a12a-6f14d05c21ea" />
Used URLhaus.abuse platform to check the URL 

<img width="953" height="397" alt="Screenshot 2026-03-20 224240" src="https://github.com/user-attachments/assets/bacd9d97-2133-4d27-af37-f92f158d9832" />
The malicious URL that is attached in the Invoice mail. The Crytominer malware was belong to “Coinminer” Family and have some other tags also like AsyncRAT and Bitrat.

Answer: Coinminer

---

5.Identifying the specific URLs malware requests is key to disrupting its communication channels and reducing its impact. Based on the previous analysis of the cryptocurrency malware sample, what does this malware request the URL?

To find out what URL Request that malware was sending, We need to do OSINT on VirusTotal. Checked on the Relations field and in Communication Files there is install.exe which was the malicious file.

<img width="958" height="477" alt="Screenshot 2026-03-20 225637" src="https://github.com/user-attachments/assets/d8ac6dfe-e5ea-48db-96ca-10bedfdafb22" />
Clicked on that to view more detail about that file. Found a URL that the file install.exe is making a contact in the Contact URLs Field.

<img width="960" height="478" alt="Screenshot 2026-03-20 225918" src="https://github.com/user-attachments/assets/58d9c41e-f8eb-4a76-9c5e-a79311d71927" />
Answer: hxxp[://]ripley[.]studio/loader/uploads/Qanjttrbv[.]jpeg

---

6.Understanding the registry entries added to the auto-run key by malware is crucial for identifying its persistence mechanisms. Based on the BitRAT malware sample analysis, what is the executable's name in the first value added to the registry auto-run key?

The question is now asking executable name for the BitRAT malware. I took the hash value form the URLhaus.

```jsx
BitRat:
bf7628695c2df7a3020034a065397592a1f8850e59f9a448b555bc1c8c639539
```

And paste it on the VirusTotal to Analysis it. Gone to community section and found a full report on vmray site.

- https://www.vmray.com/analyses/_vt/bf7628695c2d/report/overview.html

In that the question is asking for the name of the executable in the persistence phase of the malware.

<img width="959" height="476" alt="Screenshot 2026-03-20 232338" src="https://github.com/user-attachments/assets/b02f78cd-1b72-4f0b-b216-01774f18bfc7" />
In the Persistence section there is a executable processes called “ Jzwvix.exe ”

Answer: Jzwvix.exe

---

7.Identifying the SHA-256 hash of files downloaded from a malicious URL is essential for tracking and analyzing malware activity. Based on the BitRAT analysis, what is the SHA-256 hash of the file previously downloaded and added to the autorun keys?

During the pervious question i have already took a copy of the SHA256 hash value of the BitRAT from the site URLhaus.

Answer: bf7628695c2df7a3020034a065397592a1f8850e59f9a448b555bc1c8c639539

---

8.Analyzing the HTTP requests made by malware helps in identifying its communication patterns. What is the URL in the HTTP request used by the loader to retrieve the BitRAT malware?
In the vmray found out that bitrat is communicating with the IP address of “ 107.175.247.199 ”

<img width="960" height="475" alt="Screenshot 2026-03-20 233340" src="https://github.com/user-attachments/assets/838e1c3b-b903-4659-b0fa-c2b25a2396b9" />
Going back to the virustotal to find the connection url on the IP Address.

<img width="960" height="468" alt="Screenshot 2026-03-20 233136" src="https://github.com/user-attachments/assets/a7a5819d-f48a-4e85-9b17-acfcf7725eba" />
Answer: hxxp[://]107[.]175[.]247[.]199/loader/server[.]exe

---

9.Introducing a delay in malware execution can help evade detection mechanisms. What is the delay (in seconds) caused by the PowerShell command according to the BitRAT analysis?
Anti-sleep mechanisms in malware are techniques designed to prevent a computer, mobile device, or virtualized environment from entering a low-power or dormant state. This allows the malware to maintain persistence, ensure continuous data exfiltration, or prevent the interruption of malicious tasks.

<img width="960" height="479" alt="Screenshot 2026-03-21 121726" src="https://github.com/user-attachments/assets/4c6aa277-7e31-4b33-9887-8735d1e7f582" />
Anti-Sleep Triggered (0x0200000E): The overall sleep time of all monitored processes was truncated from "50 seconds" to "10 seconds" to reveal dormant functionality.

Answer: 50

---

10.Tracking the command and control (C2) domains used by malware is essential for detecting and blocking malicious activities. What is the C2 domain used by the BitRAT malware?
Searched the details about C2 domain in Joe sandbox and vmray reports. But  there is not details about C2 in the reports. 

Hatching Triage:

- https://tria.ge/221026-gxvytsehdp

The above report contains the details about the C2 Domain

<img width="945" height="474" alt="Screenshot 2026-03-21 122923" src="https://github.com/user-attachments/assets/a520ad5d-0c20-456e-a0e5-5129dabafe6c" />
Answer: gh9st[.]mywire[.]org

---

11.Understanding how malware exfiltrates data is essential for detecting and preventing data breaches. According to the AsyncRAT analysis, what is the Telegram Bot ID used by this malware?
Now we have to analysis the AsyncRat. So i have collected the hash value from the URLhaus site.

```jsx
AsynRAT:
5ca468704e7ccb8e1b37c0f7595c54df4e2f4035345b6e442e8bd4e11c58f791
```

In the community section took a report and searched the network sections to find the telegram id. 

Hatching Triage:

https://tria.ge/221025-m4mhxscdep/behavioral2

In the Network Sections contains the details about telegram id

<img width="960" height="472" alt="Screenshot 2026-03-21 124447" src="https://github.com/user-attachments/assets/ce9c9a61-d01c-4aa8-a318-b3af43d16ecc" />
Answer: bot5610920260

## Author

### RUTHRAN-SEC
