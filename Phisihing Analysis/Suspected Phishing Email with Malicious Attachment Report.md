# Suspected Phishing Email with Malicious Attachment

## **Phishing Analysis**

## Blue Team Lab Online

### Tools Used: Text Editor, Mozilla Thunderbird, URL2PNG, WHOis

### Scenario

You are working as a SOC Analyst in a newly established Security Operations Center that is still in the process of building its detection and response capabilities. As part of threat intelligence gathering and SOC maturity improvement, you are tasked with reviewing a threat intelligence report released in 2022.

The objective is to analyze the report, identify relevant threat trends, attacker tactics, techniques, and procedures (TTPs), and suggest actionable outcomes that can be implemented within the SOC. These outcomes may include improvements to detection rules, monitoring use cases, incident response playbooks, and analyst awareness to better prepare the SOC against emerging threats.

## **Alert: Emerging Threat Trends Identified from 2022 Threat Intelligence Report**

Got a file name: be3c0ced06b383dc321e49d1122cf0e53f8cf5db.zip and unzip the file 

and got Website contact form submission.eml.

Using Thunderbird to open the email.

### Challenge Question

1.Who is the primary recipient of this email? 

<img width="694" height="294" alt="Screenshot 2026-01-15 180430" src="https://github.com/user-attachments/assets/8228c7d4-cbb4-476b-8a97-b10d4d8cae9b" />

Answer: kinnar1975@yahoo.co.uk

2.What is the subject of this email? 

<img width="726" height="295" alt="Screenshot 2026-01-15 180550" src="https://github.com/user-attachments/assets/43f918ce-50d7-4fa3-8d4c-eaaf8f262287" />

Answer: Undeliverable: Website contact form submission

3.What is the date and time the email was sent? 

<img width="669" height="290" alt="Screenshot 2026-01-15 180642" src="https://github.com/user-attachments/assets/6001e185-be48-409e-923b-1044b0bb352f" />

Answer: 18 March 2021 04:14

4.What is the Originating IP? 

In the Thunderbird right side top, There is “more” button. Click that to see the View source option. Click to view the full source of the email. In that search for the Originating by pressing ctrl+f to search the specific word in it. 

<img width="714" height="418" alt="Screenshot 2026-01-15 181157" src="https://github.com/user-attachments/assets/b5701d2e-3b44-40be-baee-26481aa86011" />

Answer: 103.9.171.10

5.Perform reverse DNS on this IP address, what is the resolved host? 

To reverse the DNS on the IP we have to use the tool Whois. It is a online tool 

<img width="917" height="391" alt="Screenshot 2026-01-15 182513" src="https://github.com/user-attachments/assets/504778e6-6903-4c5f-8ac9-b07904ca3b8f" />

Answer: *c5s2–1e-syd.hosting-services.net.au*

6.What is the name of the attached file? 

<img width="960" height="341" alt="Screenshot 2026-01-15 183400" src="https://github.com/user-attachments/assets/a0f3316d-6ea1-4993-9a45-7b41b445a974" />

Answer: Website contact form submission.eml

7.What is the URL found inside the attachment? 

<img width="882" height="283" alt="Screenshot 2026-01-15 183903" src="https://github.com/user-attachments/assets/60f33267-b2c6-402e-8aa1-a377324e1205" />

Answer:https://35000usdperwwekpodf.blogspot.sgp=9swghttps://35000usdperwwekpodf.blogspot.co.il?o=0hnd

8.What service is this webpage hosted on? 

<img width="725" height="290" alt="Screenshot 2026-01-15 184212" src="https://github.com/user-attachments/assets/38e8761f-753d-4151-88d1-c4471dee24bd" />

Answer: blogspot

9.Using URL2PNG, what is the heading text on this page? (Doesn't matter if the page has been taken down!) 

Using URL2PNG Tool to capture the screen shot of that site 

<img width="960" height="476" alt="Screenshot 2026-01-15 184521" src="https://github.com/user-attachments/assets/6fbab359-26ec-4062-8c0e-ce6dd15e82fb" />

Answer: Blog has been removed

### Done By Ruthran-sec
