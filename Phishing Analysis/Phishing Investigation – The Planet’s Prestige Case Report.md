## Blue Team Online Lab

### Tools Used: Thunderbird, Text Editor

## Forensic analysis and threat investigation.

## **Scenario**

CoCanDa, known as “The Heaven of the Universe,” is facing widespread unrest due to unexplained abductions of its citizens. After the Planetary President’s daughter goes missing, a CoCanDa Army Major stationed on Earth receives a suspicious email. The email may be linked to the abductions, requiring immediate forensic analysis and threat investigation.

## Alert

Suspicious external email received by a high-ranking official, potentially related to an ongoing abduction campaign. The email may contain malicious links, attachments, or embedded payloads intended for phishing or initial access.

### Challenge Questions

1.What is the email service used by the malicious actor? 

To see click more on right top and click view source. The email was received from the local host server.

<img width="960" height="414" alt="Screenshot 2026-02-14 173437" src="https://github.com/user-attachments/assets/c78b50f6-51d8-446f-b6a3-0edbdc39a7b8" />

Answer: emkei.cz

---

2.What is the Reply-To email address? 

Use Search feature to search “reply” and see “reply-to”

<img width="958" height="411" alt="Screenshot 2026-02-14 174022" src="https://github.com/user-attachments/assets/c803b47d-50de-434b-83ba-fefbcd5f7bdb" />

Answer: negeja3921@pashter.com

---

3.What is the filetype of the received attachment which helped to continue the investigation? 

The Content in the email was encoded with base64 and mentioned filename="PuzzleToCoCanDa.pdf”

but that can be modified. So we use cyber chef to decode base64 and make it hex. And copy the first four bytes to check the file signature. 

<img width="960" height="474" alt="Screenshot 2026-02-14 175320" src="https://github.com/user-attachments/assets/26fea768-3eff-4358-9eb5-6a1d3d2f649f" />

Use garykessler platform to search for the file signature 

<img width="960" height="430" alt="Screenshot 2026-02-14 175653" src="https://github.com/user-attachments/assets/6fe89f0b-3d2f-4b43-b982-25bdf8f802fc" />

Answer: .zip

---

4.What is the name of the malicious actor? 

Pasting the base54 “PuzzleToCoCanDa.pdf” in cyber chef and Downloading it. Inside that there is a folder name ‘PuzzleToCoCanDa” which contains three files:

- DaughtersCrown - JPEG
- GoodJobMajor - PDF
- Money.xlsx

Used file command to know the file type (extension)

<img width="959" height="136" alt="Screenshot 2026-02-14 185515" src="https://github.com/user-attachments/assets/c4abd3b4-676d-48d5-a0ad-0313e64b0e67" />

I have used pdfinfo tool on GoodJobMajor pdf file to know who is the author of the file 
<img width="726" height="316" alt="Screenshot 2026-02-14 190047" src="https://github.com/user-attachments/assets/c5104a29-7f01-489c-9191-a244dd5dd65c" />


Answer:  Pestero Negeja

---

5.What is the location of the attacker in this Universe?

The Goodjomajor.pdf had the message “Location to send 1 Billion CoCanDs is in Money.xlsx”

Opened the Money.xlsx file in Excel. There are two sheets Total

<img width="939" height="331" alt="Screenshot 2026-02-14 190724" src="https://github.com/user-attachments/assets/31c1f5b4-f227-4900-be86-0797655586f5" />

There was nothing in the sheet one after doing clear direct formatting. The sheet two was empty so i again done clear direct formatting and saw a base64 encoded one

<img width="925" height="335" alt="Screenshot 2026-02-14 191043" src="https://github.com/user-attachments/assets/4fbedec3-7c71-4e1d-9105-f51a5bcd2fcf" />

Decoded → The Martian Colony, Beside Interplanetary Spaceport. This was the location of the attackers universe

Answer: The Martian Colony, Beside Interplanetary Spaceport.

---

6.What could be the probable C&C domain to control the attacker’s autonomous bots? 

Done OSINT on each domain in the email. Used VirusTotal 
<img width="960" height="475" alt="Screenshot 2026-02-14 192217" src="https://github.com/user-attachments/assets/d9833f15-9b07-4ff7-98a3-2ee92e303afd" />

<img width="960" height="475" alt="Screenshot 2026-02-14 192217" src="https://github.com/user-attachments/assets/b7079a1a-1566-4813-8e19-983109b0dccb" />


Answer: pashter.com
