# Tusk Infostealer Investigation – Credential Theft & Cryptocurrency Drain Analysis

## Cyber Defender

## Scenario

A blockchain development company identified suspicious activity after an employee was unexpectedly redirected to an unfamiliar website while accessing a DAO management platform. Shortly after the incident, multiple organizational cryptocurrency wallets were drained of funds.

Initial findings suggest the use of an infostealer malware designed to harvest credentials and exfiltrate sensitive data.

As part of the investigation, your objectives are to:

- Analyze the suspected infostealer behavior
- Identify the initial infection vector (malicious redirect)
- Extract Indicators of Compromise (IOCs)
- Trace attacker-controlled infrastructure
- Understand how credentials were harvested and abused
- Correlate activity leading to cryptocurrency theft

The goal is to uncover the full attack chain and support containment, attribution, and prevention efforts.

## Alert

**Security Alert: Suspicious Credential Theft & Cryptocurrency Drain Activity**

Unusual activity was detected following a user session involving a DAO management platform, where the user was redirected to an untrusted external website.

Shortly after, multiple cryptocurrency wallets associated with the organization were compromised and funds were transferred to unknown addresses.

Indicators suggest the use of credential-stealing malware capable of:

- Harvesting browser-stored credentials
- Capturing session tokens or wallet access data
- Communicating with external command-and-control infrastructure

Immediate investigation is required to identify the malware, trace attacker activity, and prevent further financial loss.

## Tools Used

- Virus total
- sdvs

## Given Files

```jsx
MD5: E5B8B2CF5B244500B22B665C87C11767
```

## Challenge Questions

1.In **KB**, what is the size of the malicious file?
For Analyzing the give md5 hash value, using Virustotal platform to perform OSINT on the file.

<img width="960" height="472" alt="Screenshot 2026-05-05 131525" src="https://github.com/user-attachments/assets/b1bdbf4b-3544-457b-a95d-be48f69410e5" />

The file size was 921.36 KB

Answer: 921.36

---

2.What word do the threat actors use in log messages to describe their victims, based on the name of an ancient hunted creature?
Refer Link:https://securelist.com/tusk-infostealers-campaign/113367/

We identified three active sub-campaigns (at the time of analysis) and 16 inactive sub-campaigns related to this activity. We dubbed it “Tusk”, as the threat actor uses the word “Mammoth” in log messages of initial downloaders — at least in the three active sub-campaigns we analyzed. “Mammoth” is slang used by Russian-speaking threat actors to refer to victims. Mammoths used to be hunted by ancient people and their tusks were harvested and sold.

<img width="960" height="464" alt="Screenshot 2026-05-05 132006" src="https://github.com/user-attachments/assets/f37c953a-50ec-49a7-919f-67d6b3057612" />

Answer: Mammoth

---

3.The threat actor set up a malicious website to mimic a platform designed for creating and managing decentralized autonomous organizations (DAOs) on the MultiversX blockchain (peerme.io). What is the name of the malicious website the attacker created to simulate this platform?

Refer Link: https://securelist.com/tusk-infostealers-campaign/113367/

In this campaign the actor simulated peerme[.]io, a platform for the creation and management of decentralized autonomous organizations (DAOs) on the MultiversX blockchain. It aims to empower crypto communities and projects by providing tools for governance, funding, and collaboration within a decentralized framework. The malicious website is tidyme[.]io.

<img width="613" height="473" alt="Screenshot 2026-05-05 132335" src="https://github.com/user-attachments/assets/4efbc3b3-fc37-4313-b800-77ee1ed4db50" />

Answer: tidyme.io

---

4.Which cloud storage service did the campaign operators use to host malware samples for both macOS and Windows OS versions?

Refer Link: https://securelist.com/tusk-infostealers-campaign/113367/

This campaign has several malware samples for macOS and Windows, both hosted on Dropbox. In this post we will explore Windows samples only.

<img width="616" height="298" alt="Screenshot 2026-05-05 133119" src="https://github.com/user-attachments/assets/761b3188-72c6-4f0c-86de-1689f4be99ce" />

Answer: Dropbox

---

5.The malicious executable contains a configuration file that includes base64-encoded URLs and a password used for archived data decompression, enabling the download of second-stage payloads. What is the password for decompression found in this configuration file?

Refer Link: https://securelist.com/tusk-infostealers-campaign/113367/

The **tidyme.exe** sample contains a configuration file called **config.json** which contains base64-encoded URLs and a password for archived data decompression, which is used to download the second-stage payloads. Here is the content of the file:

```jsx
{
 "archive": "aHR0cHM6Ly93d3cuZHJvcGJveC5jb20vc2NsL2ZpL2N3NmpzYnA5ODF4eTg4dHprM29ibS91cGRhdGVsb2FkLnJhcj9ybGtleT04N2c5NjllbTU5OXZub3NsY2dseW85N2ZhJnN0PTFwN2RvcHNsJmRsPTE=",
 "password": "newfile2024",
 "bytes": "aHR0cDovL3Rlc3Rsb2FkLnB5dGhvbmFueXdoZXJlLmNvbS9nZXRieXRlcy9m"
}
```

<img width="618" height="464" alt="Screenshot 2026-05-05 134320" src="https://github.com/user-attachments/assets/a73a97c6-0aaf-4eed-a5e4-79baee44720d" />

Answer: newfile2024

---

6.What is the name of the function responsible for retrieving the field archive from the configuration file?

Refer Link: https://securelist.com/tusk-infostealers-campaign/113367/

The main downloader functionality is stored in preload.js file in two functions, downloadAndExtractArchive and loadFile. The function downloadAndExtractArchive retrieves the field archive from the configuration file, which is an encoded Dropbox link, decodes it and stores the file from Dropbox to the path %TEMP%/archive-<RANDOM_STRING>. The downloaded file is a password-protected RAR file which will be extracted with the value of the field password in the configuration file, then all .exe files from this archive are executed.

Answer: downloadAndExtractArchive

---

7.In the third sub-campaign carried out by the operators, the attacker mimicked an AI translator project. What is the name of the legitimate translator, and what is the name of the malicious translator created by the attackers?

In this campaign, the threat actor was simulating an AI translator project named **YOUS**. The original website is **yous.ai**, while the malicious website is **voico[.]io**:

<img width="1024" height="306" alt="Tusk-campaign-screen-en-ru-es_06-1024x306" src="https://github.com/user-attachments/assets/bacaded2-452a-4d6c-90c8-302825d65c77" />

Just like the previous two sub-campaigns, the malicious website contains a download link for the initial downloader imitating the application. The downloader is hosted on Dropbox and follows the same logic described in the first sub-campaign to download the appropriate downloader for the victim’s operating system. During our investigation, the malicious website of this campaign ceased to exist. The sample name is **Voico.exe**.

Answer: yous[.]ai , voico[.]io

---

8. The downloader is tasked with delivering additional malware samples to the victim’s machine, primarily infostealers like StealC and Danabot. What are the IP addresses of the **StealC C2 servers** used in the campaign?

<img width="592" height="188" alt="Screenshot 2026-05-05 140349" src="https://github.com/user-attachments/assets/5396932b-15ef-4b3f-a1c2-1e08a28e9d69" />

Answer: 46.8.238[.]240, 23.94.225[.]177

---

9.What is the address of the Ethereum cryptocurrency wallet used in this campaign?

This downloader is responsible for delivering additional malware samples to the victim’s machine, which are mostly infostealers (**Danabot** and **StealC**) and clippers. Besides this, the actors use phishing to trick users into providing additional sensitive information, such as credentials, which can then be sold on the dark web or used to gain unauthorized access to their gaming accounts and cryptocurrency wallets and drain their funds directly.

<img width="578" height="212" alt="Screenshot 2026-05-05 141156" src="https://github.com/user-attachments/assets/65e46df3-b353-45a8-b465-20c900e03b2f" />

In addition to distributing malware, this campaign involves victims connecting their cryptocurrency wallets directly through the campaign’s website. To investigate further, we created a test wallet with a small balance and linked it to the site. However, no withdrawal transactions were initiated in the course of this study. The purpose of this action was to expose the threat actor’s cryptocurrency wallet address for subsequent blockchain analysis.

Answer: 0xaf0362e215Ff4e004F30e785e822F7E20b99723A

---

## Author

### RUTHRAN-SEC
