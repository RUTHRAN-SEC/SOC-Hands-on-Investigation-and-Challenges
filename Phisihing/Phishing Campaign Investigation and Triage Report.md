# Phishing Campaign Investigation and Triage

## **Phishing Analysis 2**

## Blue Team lab Online

### Tools Used: Text Editor, Thunderbird

### Scenario

You are working as a SOC Analyst in a newly established Security Operations Center that is still in the process of building its detection and response capabilities. As part of threat intelligence gathering and SOC maturity improvement, you are tasked with reviewing a threat intelligence report released in 2022.

The objective is to analyze the report, identify relevant threat trends, attacker tactics, techniques, and procedures (TTPs), and suggest actionable outcomes that can be implemented within the SOC. These outcomes may include improvements to detection rules, monitoring use cases, incident response playbooks, and analyst awareness to better prepare the SOC against emerging threats.

## **Alert: Emerging Threat Trends Identified from 2022 Threat Intelligence Report**

Got 32382fd7399b5de6e415ffe1b94b3d5d640a09bd.zip, unzip and we get Your_Account_has_been_locked.eml → It’s an email file, I used Thunderbird to open the email 

### **Challenge Question**

1.What is the sending email address? 

<img width="960" height="361" alt="Screenshot 2026-01-15 213116" src="https://github.com/user-attachments/assets/45a9814d-cde6-48b8-983e-7831cb5316a4" />

Answer: amazon@zyevantoby.cn

2.What is the recipient email address? 

<img width="775" height="344" alt="Screenshot 2026-01-15 213349" src="https://github.com/user-attachments/assets/effe6c2d-a22c-41e2-994e-c4638c7e69e7" />

Answer: saintington73@outlook.com

3.What is the subject line of the email? 

<img width="960" height="338" alt="Screenshot 2026-01-15 214043" src="https://github.com/user-attachments/assets/f74c9fbd-4ca1-40d5-8822-d433c954f284" />

Answer: Your Account has been locked

4.What company is the attacker trying to imitate?

Amazon

<img width="714" height="335" alt="Screenshot 2026-01-15 214702" src="https://github.com/user-attachments/assets/3d2f5bc3-632d-4f60-96a6-3c65d6dd9111" />

Answer: Amazon

5.What is the date and time the email was sent?

More → View Source

<img width="804" height="408" alt="Screenshot 2026-01-15 215407" src="https://github.com/user-attachments/assets/6480cb3b-b9d2-48a8-baec-579c3b11a8aa" />

Answer: *Wed, 14 Jul 2021 01:40:32 +0900*

6.What is the URL of the main call-to-action button?

Review Account → Action button

<img width="960" height="417" alt="Screenshot 2026-01-15 215654" src="https://github.com/user-attachments/assets/f2c10633-89ac-4edb-9b87-05ca5001e899" />

Answer:https://emea01.safelinks.protection.outlook.com/url=https%3A%2F%2Famaozn.zzyuchengzhika.cn%2F%3Fmailtoken%3Dsaintington73%40outlook.com&data=04|01||70072381ba6e49d1d12d08d94632811e|84df9e7fe9f640afb435aaaaaaaaaaaa|1|0|637618004988892053|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|1000&sdata=oPvTW08ASiViZTLfMECsvwDvguT6ODYKPQZNK3203m0%3D&reserved=0

7.Look at the URL using URL2PNG. What is the first sentence (heading) displayed on this site? 

The URL2PNG is not working properly so i used separate VM environment to browser it 

<img width="960" height="354" alt="Screenshot 2026-01-15 220645" src="https://github.com/user-attachments/assets/dba49d08-a4f5-40ee-9283-647cbedc79bf" />

Answer: This web page could not be loaded.

8.When looking at the main body content in a text editor, what encoding scheme is being used?

<img width="601" height="418" alt="Screenshot 2026-01-15 221050" src="https://github.com/user-attachments/assets/3894e794-54df-47e3-a035-59eec0f6e514" />

Answer: Base64

9.What is the URL used to retrieve the company's logo in the email?

The First Base64 encoded was a content of the email

![1_AKqLTd-YozrD71RI2K2F-g](https://github.com/user-attachments/assets/c6b84d10-cbbf-4a3a-860e-6c71e087514d)

![1_KHiBNVQ48hrADpEVSs5_sA](https://github.com/user-attachments/assets/aab7adbf-e17c-4745-967d-c8871d1c8324)

Answer: *https://images.squarespace-cdn.com/content/52e2b6d3e4b06446e8bf13ed/1500584238342-OX2L298XVSKF8AO6I3SV/amazon-logo?format=750w&amp;content-type=image%2Fpng*

10.For some unknown reason one of the URLs contains a Facebook profile URL. What is the username (not necessarily the display name) of this account, based on the URL? 

Search for the word “Facebook” to find the Username

![1_BvLxkY9W0IoDMb7jTWwUHQ](https://github.com/user-attachments/assets/d6202f54-ec9a-4169-b60b-4936b9871275)

Answer: *amir.boyka.7*

### Done By Ruthran-sec
