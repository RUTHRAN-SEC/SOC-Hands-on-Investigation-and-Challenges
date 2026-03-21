# Deleted File Recovery Investigation – Endpoint Forensic Analysis

## Blue Team Lab Online

## Scenario

An employee at FakeCompany Ltd., recently awarded “Best Employee of the Year,” accidentally deleted critical business files from his workstation. The deleted data contained sensitive and operationally important information.

As part of the incident response and digital forensics team, you are tasked with:

- Recovering deleted files from the system
- Identifying how and when the files were deleted
- Determining whether deletion was accidental or intentional
- Extracting and preserving relevant artifacts
- Documenting forensic findings

The objective is to restore business-critical data and ensure no malicious activity was involved.

---

## Alert

**Critical File Deletion Alert:** Multiple important business files were reported missing from an employee workstation. Initial review suggests that the files were deleted from the local system, potentially impacting operational workflows.

A forensic investigation has been initiated to recover deleted data, analyze file system artifacts, and determine whether the deletion was accidental or indicative of suspicious activity. Immediate analysis is required to restore files and assess potential risk.

## Tools Uesd

- foremost

## Given Files

```jsx
10240 -rw-rw-r-- 1 root root 10485760 Feb 25  2021 recoverfiles.dd
```

## Challenge Question

1.What is the text written on the recovered gif image?

In order to recover the gif and images form the recoverfiles.dd, i am using tool called foremost.

```jsx
Command:
foremost recoverfiles.dd
```

It creates a file that what all are recovered form it.

Files We got:

```jsx
4 -rw-r--r-- 1 root root  918 Mar 21 08:47 audit.txt
4 drwxr-xr-- 2 root root 4096 Mar 21 08:47 gif/
4 drwxr-xr-- 2 root root 4096 Mar 21 08:47 mov/
4 drwxr-xr-- 2 root root 4096 Mar 21 08:47 pdf/
4 drwxr-xr-- 2 root root 4096 Mar 21 08:47 png/
4 drwxr-xr-- 2 root root 4096 Mar 21 08:47 zip/
```

<img width="832" height="340" alt="Screenshot 2026-03-21 142419" src="https://github.com/user-attachments/assets/6dcf1237-cc02-4da6-957e-805d6f8694d6" />

Answer: GoodJobDefender

---

2.Submit Flag1

The flag1 was in the png file.

<img width="899" height="392" alt="Screenshot 2026-03-21 143420" src="https://github.com/user-attachments/assets/aaff967a-a5c0-4bf0-9fa0-426b0864a4d7" />
Answer: FLAG1:WELOVEBTLO

---

3.Submit Flag2 

Checking the zip/, After extracting we got 00011120/

```jsx
root@ip-10-48-120-26:~/Downloads/i/BT/output/zip/00011120# ls -alps
total 20
4 drwxr-xr-x 4 root root 4096 Mar 21 09:11  ./
4 drwxr-xr-- 3 root root 4096 Mar 21 09:11  ../
4 -rw-rw-r-- 1 root root 1069 Feb 12  2021 '[Content_Types].xml'
4 drwxr-xr-x 2 root root 4096 Mar 21 09:11  _rels/
4 drwxr-xr-x 4 root root 4096 Mar 21 09:11  word/
root@ip-10-48-120-26:~/Downloads/i/BT/output/zip/00011120# ls -alps *
4 -rw-rw-r-- 1 root root 1069 Feb 12  2021 '[Content_Types].xml'

_rels:
total 12
4 drwxr-xr-x 2 root root 4096 Mar 21 09:11 ./
4 drwxr-xr-x 4 root root 4096 Mar 21 09:11 ../
4 -rw-rw-r-- 1 root root  298 Feb 12  2021 .rels

word:
total 40
4 drwxr-xr-x 4 root root 4096 Mar 21 09:11 ./
4 drwxr-xr-x 4 root root 4096 Mar 21 09:11 ../
4 drwxr-xr-x 2 root root 4096 Mar 21 09:11 _rels/
4 -rw-rw-r-- 1 root root 3537 Feb 12  2021 document.xml
4 -rw-rw-r-- 1 root root 1370 Feb 12  2021 fontTable.xml
4 -rw-rw-r-- 1 root root 1341 Feb 12  2021 numbering.xml
4 -rw-rw-r-- 1 root root 1770 Feb 12  2021 settings.xml
8 -rw-rw-r-- 1 root root 4575 Feb 12  2021 styles.xml
4 drwxr-xr-x 2 root root 4096 Mar 21 09:11 theme/ 
```

Gone a check each manually. While checking each i got a base64 in document.xml file.

```jsx
file:///root/Downloads/i/BT/output/zip/00011120/word/document.xml
```

<img width="960" height="313" alt="Screenshot 2026-03-21 144603" src="https://github.com/user-attachments/assets/cdfc0fd8-cdf1-4ef8-9720-7857b62d3d0b" />

```jsx
Base64: RkxBRzI6QVNPTElEREVGRU5ERVI=
Decoded: FLAG2:ASOLIDDEFENDER
```

Answer: FLAG2:ASOLIDDEFENDER

---

4.Submit Flag3 

Still i have not Analyzed the pdf/ and mov/ fully, So i am going to look at the string vale of each files. In the pdf there is a message.

<img width="850" height="393" alt="Screenshot 2026-03-21 150241" src="https://github.com/user-attachments/assets/13012e0e-9cbe-4508-9e12-1d82620d799d" />
Frist looking the strings of pdf.

Found the third flag, But it is URL encoded.

<img width="959" height="366" alt="Screenshot 2026-03-21 150428" src="https://github.com/user-attachments/assets/a12926a5-417b-4074-9017-f454f9785db5" />

```jsx
URL Encoded: FLAG3%3A%40BLU3T3AM%240LDI3R
Decoded: FLAG3:@BLU3T3AM$0LDI3R - Using CyberChef
```

Answer: FLAG3:@BLU3T3AM$0LDI3R

---

5.What is the filesystem of the provided disk image? 

Used a Tool fsstat 

```jsx
Command:
fsstat -o 2048 recoverfiles.dd
```

- **`fsstat`**: A tool from **The Sleuth Kit (TSK)** that provides general details about a file system, such as block size, inode range, and volume serial numbers.
- **`o 2048`**: This flag specifies the **offset** where the file system begins.
    - The value `2048` usually refers to **sectors**

<img width="958" height="351" alt="Screenshot 2026-03-21 151915" src="https://github.com/user-attachments/assets/7626a3ba-bc68-40eb-b83d-56a6fa4a37e7" />
Answer: ext4

---

6.What is the original filename of the recovered mp4 file? 

If i have done strings on first recover.dd file means we can complete this easily and fast.

<img width="960" height="362" alt="Screenshot 2026-03-21 152634" src="https://github.com/user-attachments/assets/8207b87c-4600-4a42-82ab-4ce6d14dc91f" />
Answer: SBTCertifications.mp4

## Author

### RUTHRAN-SEC
