# Oski Stealer Investigation – Malicious PPT File & Payload Delivery Analysis

## Cyber Defender

## Scenario

An accountant at the organization received an email with the subject “Urgent New Order,” appearing to originate from a legitimate client. Upon opening the attached invoice, it was discovered that the content was fraudulent.

Shortly after, the organization’s SIEM system generated an alert indicating the download of a potentially malicious file. Initial triage revealed that a PowerPoint (PPT) document may have triggered the download of a secondary payload.

You are tasked with performing a detailed analysis of the suspicious document to:

- Identify embedded malicious content within the PPT file
- Determine how the payload was delivered
- Analyze the downloaded file and identify the malware family
- Extract Indicators of Compromise (IOCs)
- Understand the full attack chain from phishing to execution

The goal is to confirm the infection vector and support incident response efforts.

## Alert

**SOC Alert: Suspicious File Download Triggered by Document Execution**

Security monitoring detected a potentially malicious file download following the opening of an email attachment by a user. The attachment, disguised as an invoice, appears to have initiated unauthorized network activity.

Observed indicators include:

- Execution of a document file triggering external connections
- Download of a suspicious payload from a remote source
- Potential credential theft or data exfiltration activity
- Indicators consistent with known infostealer malware

Immediate analysis is required to determine the nature of the file and prevent further compromise.

## Tools used

- VirusTotal
- ANY.RUN

## Given File

```jsx
MD5 Hash: 12c1842c3ccafe7408c23ebf292ee3d9
```

## Challenge Questions

1.Determining the creation time of the malware can provide insights into its origin. What was the time of malware creation?
Using the Virus Total platform for Static Analysis and perform OSINT on the hash value.

<img width="317" height="132" alt="Screenshot 2026-05-06 163045" src="https://github.com/user-attachments/assets/7db62dbe-b81e-430f-836e-a6e2c3cd1f96" />

Answer: 2022-09-28 17:40:46 UTC

---

2.Identifying the command and control (C2) server that the malware communicates with can help trace back to the attacker. Which C2 server does the malware in the PPT file communicate with?

<img width="691" height="104" alt="Screenshot 2026-05-06 163541" src="https://github.com/user-attachments/assets/c323ba43-c0ec-44e4-bdaf-d2d1fbf40331" />

Answer: hxxp[://]171[.]22[.]28[.]221/5c06c05b7b34e8e6[.]php

---

3. Identifying the initial actions of the malware post-infection can provide insights into its primary objectives. What is the first library that the malware requests post-infection?

<img width="455" height="277" alt="Screenshot 2026-05-06 164404" src="https://github.com/user-attachments/assets/635a07e0-3af3-4ead-81f1-53ab927415e9" />

Answer: sqlite3.dll

---

4.By examining the provided Any run report, what RC4 key is used by the malware to decrypt its base64-encoded string?
Provided Any run report: https://any.run/report/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/d55e2294-5377-4a45-b393-f5a8b20f7d44

<img width="846" height="203" alt="Screenshot 2026-05-06 203102" src="https://github.com/user-attachments/assets/b13b7ffa-7ab8-455d-9b66-b2038156b2a1" />

Answer: 5329514621441247975720749009

---

5.By examining the MITRE ATT&CK techniques displayed in the Any.run sandbox report, identify the main MITRE technique (not sub-techniques) the malware uses to steal the user’s password.

Any.run sandbox report https://app.any.run/tasks/d55e2294-5377-4a45-b393-f5a8b20f7d44

**Credentials from Password Stores**

<img width="537" height="373" alt="Screenshot 2026-05-06 203632" src="https://github.com/user-attachments/assets/7538fdeb-4e46-4ab2-81b5-a00f54fd572a" />

Adversaries may search for common password storage locations to obtain user credentials.(Citation: F-Secure The Dukes) Passwords are stored in several places on a system, depending on the operating system or application holding the credentials. There are also specific applications and services that store passwords to make them easier for users to manage and maintain, such as password managers and cloud secrets vaults. Once credentials are obtained, they can be used to perform lateral movement and access restricted information.

Answer: T1555

---

6.By examining the child processes displayed in the Any.run sandbox report, which directory does the malware target for the deletion of all **DLL** files?

Any.run sandbox report: https://app.any.run/tasks/d55e2294-5377-4a45-b393-f5a8b20f7d44

<img width="848" height="204" alt="Screenshot 2026-05-06 204353" src="https://github.com/user-attachments/assets/5bfefd6a-0a51-48cb-9e47-5726aefde43a" />

Answer: C:\ProgramData

---

7.Understanding the malware's behavior post-data exfiltration can give insights into its evasion techniques. By analyzing the child processes, after successfully exfiltrating the user's data, **how many seconds does** it take for the malware to **self-delete**?

Refer: https://any.run/report/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/d55e2294-5377-4a45-b393-f5a8b20f7d44

**Starts CMD.EXE for self-deleting**

- VPN.exe (PID: 3484)
- C:\Windows\System32\timeout.exe

<img width="859" height="119" alt="Screenshot 2026-05-06 210347" src="https://github.com/user-attachments/assets/f231d80f-2da7-468f-acf1-273d8b769130" />

Answer: 5

---

## Author

### RUTHRAN-SEC
