# Android Mobile Forensics Investigation Report

## Cyber Defender

## Scenario

As part of an active homicide investigation, investigators recovered the victim’s Android mobile device as critical digital evidence. Interviews conducted with witnesses and individuals close to the victim revealed several leads that may help reconstruct the events surrounding the incident.

You have been assigned to perform a detailed forensic analysis of the device using Android artifact analysis tools. Your objectives are to:

- Recover and analyze communication records
- Examine financial and application-related artifacts
- Reconstruct the victim’s movements and activity timeline
- Correlate evidence from multiple Android data sources
- Identify suspicious interactions or anomalies prior to the incident

The goal is to establish a clear sequence of events leading up to the crime and provide actionable forensic findings to investigators.

## Alert

**Digital Forensics Investigation Alert:** Android mobile device acquired as evidence in an active homicide investigation. Investigators require detailed analysis of device artifacts to reconstruct the victim’s communications, financial activity, and movements prior to the incident.

Preliminary findings indicate the device may contain critical evidence related to timeline reconstruction and interactions with individuals connected to the case. Immediate forensic analysis is required to preserve and correlate digital evidence.

## Tools Used

- ALEAPP
- DB Browser for SQLite

## Given Files

```jsx
total 140
4 drwx------   2 root root 4096 Sep 20  2023 adb/
4 drwx------   2 root root 4096 Sep 20  2023 anr/
4 drwx------   4 root root 4096 Sep 20  2023 app/
4 drwx------   2 root root 4096 Sep 20  2023 app-asec/
4 drwx------   2 root root 4096 Sep 20  2023 app-ephemeral/
4 drwx------   2 root root 4096 Sep 20  2023 app-lib/
4 drwx------   2 root root 4096 Sep 20  2023 app-private/
4 drwx------   6 root root 4096 Sep 20  2023 backup/
4 drwx------   2 root root 4096 Sep 20  2023 bootchart/
4 drwx------   5 root root 4096 Sep 20  2023 cache/
4 drwx------ 112 root root 4096 Sep 21  2023 data/
4 drwx------   3 root root 4096 Sep 20  2023 drm/
4 drwx------   4 root root 4096 Sep 20  2023 local/
4 drwx------   2 root root 4096 Sep 20  2023 lost+found/
4 drwx------   4 root root 4096 Sep 20  2023 media/
4 drwx------   2 root root 4096 Sep 20  2023 mediadrm/
4 drwx------  44 root root 4096 Sep 20  2023 misc/
4 drwx------   3 root root 4096 Sep 20  2023 misc_ce/
4 drwx------   3 root root 4096 Sep 20  2023 misc_de/
4 drwx------   4 root root 4096 Sep 20  2023 nativetest/
4 drwx------   3 root root 4096 Sep 20  2023 nfc/
4 drwx------   2 root root 4096 Sep 20  2023 ota/
4 drwx------   2 root root 4096 Sep 20  2023 ota_package/
4 drwx------   2 root root 4096 Sep 20  2023 property/
4 drwx------   2 root root 4096 Sep 20  2023 resource-cache/
4 drwx------   2 root root 4096 Sep 20  2023 ss/
4 drwx------  19 root root 4096 Sep 20  2023 system/
4 drwx------   3 root root 4096 Sep 20  2023 system_ce/
4 drwx------   3 root root 4096 Sep 20  2023 system_de/
4 drwx------   2 root root 4096 Sep 20  2023 tombstones/
4 drwx------   2 root root 4096 Sep 20  2023 user/
4 drwx------   3 root root 4096 Sep 20  2023 user_de/
4 drwx------   7 root root 4096 Sep 20  2023 vendor/
4 drwx------   3 root root 4096 Sep 20  2023 vendor_ce/
4 drwx------   3 root root 4096 Sep 20  2023 vendor_de/
```

## Challenge Questions

1.Based on the accounts of the witnesses and individuals close to the victim, it has become clear that the victim was interested in trading. This has led him to invest all of his money and acquire debt. Can you identify the **`SHA256`** of the trading application the victim primarily used on his phone?

Form the given database, i have used the find command to search for the trading app using the word “trade”.

```jsx
find . -name "*trade*" 2>/dev/null
```

<img width="503" height="247" alt="Screenshot 2026-05-08 135042" src="https://github.com/user-attachments/assets/8f83ee36-e900-4fd5-bdad-e8f70d96344d" />

Found the full path the app was located and moves to the directory of `com.ticno.olymptrade-lKDfBXc8qLNF9F2eXSyBwg==`

And found out the app was in the name of base.apk

Used the command sha256sum to find the hash value.

Answer: 4F168A772350F283A1C49E78C1548D7C2C6C05106D8B9FEB825FDC3466E9DF3C

---

2.According to the testimony of the victim's best friend, he said, "**`While we were together, my friend got several calls he avoided. He said he owed the caller a lot of money but couldn't repay now`**". How much does the victim owe this person?

In order to find how much does the victim want to repay, we have to find the folder or file that have a record of the conversation that the victim had. Used the word “sms” to search for the folder using the command “find”.  

```jsx
find . -name '*sms*' 2>/dev/null
./data/com.google.android.gms/shared_prefs/proxy-sms-corpus.xml
./data/com.google.android.gms/shared_prefs/ipa-sms-corpus.xml
./data/com.google.android.gms/databases/icing_mmssms.db-shm
./data/com.google.android.gms/databases/icing_mmssms.db-wal
./data/com.google.android.gms/databases/icing_mmssms.db
./data/com.google.android.gms/databases/ipa_mmssms.db
./data/com.android.smspush
./misc/sms
./misc/profiles/ref/com.android.smspush
./misc/profiles/cur/0/com.android.smspush
./user_de/0/com.android.providers.telephony/databases/mmssms.db
./user_de/0/com.android.smspush
```

The SMS was stored on the path: 

```jsx
./user_de/0/com.android.providers.telephony/databases/mmssms.db
```

Using the DB Browser for SQLite, i have search for the SMS in every table.

<img width="640" height="360" alt="Screenshot 2026-05-08 151526" src="https://github.com/user-attachments/assets/7634a869-a77e-4068-a399-1d5bdd74837f" />

**The SMS was**: l for you. Prepare the sum of 250,000 EGP, and I'll expect your call within an hour at most.

Answer: 250,000

---

3.What is the name of the person to whom the victim owes money?
The contact history must be somewhere in the database where it can help us to find the person who often made the call to the victim’s phone number.

```jsx
find . -name '*call*' 2>/dev/null
./data/com.android.calllogbackup
./data/com.android.providers.contacts/databases/calllog.db
./misc/profiles/ref/com.android.calllogbackup
./misc/profiles/cur/0/com.android.calllogbackup
./user_de/0/com.android.calllogbackup
./user_de/0/com.android.providers.contacts/databases/calllog_shadow.db
```

Form the previous question investigation i took the phone number of the person who made that message to the victim. So that we can confirm the person phone number.

**The Number:** +201172137258

Found the call logs database where there is the call history events.

```jsx
./data/com.android.calllogbackup
```

<img width="640" height="360" alt="Screenshot 2026-05-08 152704" src="https://github.com/user-attachments/assets/b343815e-02ac-43b2-8306-796a8b467d27" />

The name of the person who made the call is “Shady Wahab”. And confirmed the person by the phone number.

<img width="640" height="360" alt="Screenshot 2026-05-08 153043" src="https://github.com/user-attachments/assets/1beaa2aa-3f4a-49bb-9bab-55b9a9f5ed75" />

Confirmed the person name by the number “+201172137258”

Answer: Shady Wahab

---

4.Based on the statement from the victim's family, they said that on **`September 20, 2023`**, he departed from his residence without informing anyone of his destination. Where was the victim located at that moment?
Used the word **location** to find the victim location on the September 20, 2023.

```jsx
find . -name '*location*' 2>/dev/null
./user_de/0/com.android.location.fused - 
./misc/profiles/ref/com.android.location.fused - 
./misc/profiles/cur/0/com.android.location.fused
./data/com.android.location.fused -
./data/com.google.android.apps.maps/files/location_uploader_persistence.cs - 
```

Finding through each of the file is time consuming, So I am using the tool Aleapp ****for better analyzing the folders and files.

**The Tool Link:**  https://github.com/abrignoni/ALEAPP.git

After processing the full data. We can analyze the report.

On the recent activity the location was recorded on **Application: com.google.android.apps.maps**

<img width="520" height="227" alt="Screenshot 2026-05-09 122043" src="https://github.com/user-attachments/assets/d5b60743-ff54-4eab-9102-505cbf91a0e1" />

<img width="108" height="227" alt="Screenshot 2026-05-09 122107" src="https://github.com/user-attachments/assets/d7f293da-a11d-4468-b6f6-e67abd99a069" />

The location of the victim on September 20, 2023 was on the place called **The Nile Ritz-Carlton, Cairo.**

Answer: The Nile Ritz-Carlton

---

5.The detective continued his investigation by questioning the hotel lobby. She informed him that the victim had reserved the room for 10 days and had a flight scheduled thereafter. The investigator believes that the victim may have stored his ticket information on his phone. Look for where the victim intended to travel.

The flight tickets where found on the google photos where the victim had a ticket to go **las Vegas** 

**Google Photos (gphotos-1) - Cache located at:** 

```jsx
C:\Users\heyru\Downloads\output\ALEAPP_Reports_2026-05-09_Saturday_120645\data\data\com.google.android.apps.photos\databases\disk_cache
```

<img width="640" height="209" alt="Screenshot 2026-05-09 123611" src="https://github.com/user-attachments/assets/87b68f0f-060c-492a-81f4-2d49f10ac23b" />

Answer: **las Vegas** 

---

6.After examining the victim's Discord conversations, we discovered he had arranged to meet a friend at a specific location. Can you determine where this meeting was supposed to occur?

The meeting details of the friend was in the folder Discord Chats located at: 

```jsx
C:\Users\heyru\Downloads\output\ALEAPP_Reports_2026-05-09_Saturday_120645\data\data\com.discord\files\kv-storage\@account.665825323065016370\a
```

- The username of the person he had a meeting “rob1ns0n”
- The Meeting was discussed on the **Thursday, September 21, 2023, at 2:16 AM**.

<img width="523" height="142" alt="Screenshot 2026-05-09 124203" src="https://github.com/user-attachments/assets/9e95903d-2874-4d6d-adbf-63829a709d09" />

Answer: The Mob Museum 

---

## Author

### RUTHRAN-SEC
