# Threat Intelligence Report Review for SOC Capability Enhancement

## The Report

## Blue Team Lab Online

### Tools Used: PDFReader

### **Scenario**

You are working as a SOC Analyst in a newly established Security Operations Center that is still in the process of building its detection and response capabilities. As part of threat intelligence gathering and SOC maturity improvement, you are tasked with reviewing a threat intelligence report released in 2022.

The objective is to analyze the report, identify relevant threat trends, attacker tactics, techniques, and procedures (TTPs), and suggest actionable outcomes that can be implemented within the SOC. These outcomes may include improvements to detection rules, monitoring use cases, incident response playbooks, and analyst awareness to better prepare the SOC against emerging threats.

## **Emerging Threat Trends Identified from 2022 Threat Intelligence Report**

## Challenge Questions

1.Name the supply chain attack related to Java logging library in the end of 2021 (Format: AttackNickname) 

<img width="538" height="214" alt="Screenshot 2026-01-15 123156" src="https://github.com/user-attachments/assets/5d5e702c-dcef-41d7-809f-7e03685981b9" />

Searching the Word related “Java” in under Supply chain compromise topic 

Answer: Log4j

2.Mention the MITRE Technique ID which effected more than 50% of the customers (Format: TXXXX) 

Search using the keyword 50% for seacrhing

<img width="524" height="155" alt="Screenshot 2026-01-15 123239" src="https://github.com/user-attachments/assets/a34542ca-6eca-4ea0-95da-196a78b4f956" />

Answer: T1059

3.Submit the names of 2 vulnerabilities belonging to Exchange Servers (Format: VulnNickname, VulnNickname) 

Search using the word “Exchange Servers”

<img width="551" height="284" alt="Screenshot 2026-01-15 123605" src="https://github.com/user-attachments/assets/086532dc-2f40-4426-87be-1f24458b1929" />

<img width="449" height="280" alt="Screenshot 2026-01-15 123626" src="https://github.com/user-attachments/assets/4bdd4c94-4f65-42b2-bfca-d194734b902b" />

Answer: proxylogon:proxyshell

4.Submit the CVE of the zero day vulnerability of a driver which led to RCE and gain SYSTEM privileges (Format: CVE-XXXX-XXXXX)

Search using zero day 

<img width="457" height="207" alt="Screenshot 2026-01-15 123949" src="https://github.com/user-attachments/assets/7ea08bc7-c7cc-49c9-b1ca-0c5b27423338" />

Answer: CVE-2021-34527

5.Mention the 2 adversary groups that leverage SEO to gain initial access (Format: Group1, Group2)

Searched with “SEO”

<img width="438" height="229" alt="Screenshot 2026-01-15 125525" src="https://github.com/user-attachments/assets/7eed0f16-e4e8-47de-b006-716a8b597380" />

Answer:Gootkit,Yellow Cockatoo

6.In the detection rule, what should be mentioned as parent process if we are looking for execution of malicious js files [Hint: Not CMD] (Format: ParentProcessName.exe) 

Searched with “.js”

<img width="381" height="182" alt="Screenshot 2026-01-15 125942" src="https://github.com/user-attachments/assets/44824377-4fb1-4c22-8b8e-b709aa892ba9" />

Answer: wscript.exe

7.Ransomware gangs started using affiliate model to gain initial access. Name the precursors used by affiliates of Conti ransomware group (Format: Affiliate1, Affiliate2, Afilliate3) 

Searched with “affiliate model”

<img width="302" height="370" alt="Screenshot 2026-01-15 130526" src="https://github.com/user-attachments/assets/690f18da-e584-434f-81cb-2f95cf2f799a" />

Answer: Qbot,Bazar,IcedID

8.Question The main target of coin miners was outdated software. Mention the 2 outdated software mentioned in the report (Format: Software1, Software2) 

Searched with “outdated”

<img width="378" height="195" alt="Screenshot 2026-01-15 130916" src="https://github.com/user-attachments/assets/324f3292-be48-484a-8c8f-2919d7e8187b" />

Answer: JBoss,WebLogic

9.Name the ransomware group which threatened to conduct DDoS if they didn't pay ransom (Format: GroupName) 

Searched with “DDoS”

<img width="401" height="265" alt="Screenshot 2026-01-15 131228" src="https://github.com/user-attachments/assets/c6721f76-e70b-4bdf-8a0b-9bfe42e86c51" />

Answer: Fancy Lazarus

10.What is the security measure we need to enable for RDP connections in order to safeguard from ransomware attacks? (Format: XXX) 

Searched with “RDP”

<img width="380" height="163" alt="Screenshot 2026-01-15 131712" src="https://github.com/user-attachments/assets/2f634c09-01d3-480a-bd0a-b68c2d23c313" />

Answer: MFA

### Done By Ruthran-sec
