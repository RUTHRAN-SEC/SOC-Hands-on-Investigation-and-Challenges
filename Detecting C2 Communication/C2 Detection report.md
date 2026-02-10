# Detecting C2 Communication

## **ItsyBitsy CTF**

## TryHackme (Challenge)

### Tool Used: Elastic

### Scenario

During normal SOC monitoring, Analyst **John** observed an alert on an IDS solution indicating a potential C2 communication from a user **Browne** from the HR department. A suspicious file was accessed containing a malicious pattern THM:{  }. A week-long HTTP connection logs have been pulled to investigate. Due to limited resources, only the connection logs could be pulled out and are ingested into the `connection_logs` index in Kibana.

## Alert: Suspected Command-and-Control (C2) Communication from the user Browne from HR department.

### Challenge questions

1.How many events were returned for the month of March 2022?

Just adjusted the Date and Time settings to Mar 1, 2022 @ 00:00:00.000 - Mar 31, 2022 @ 23:30:00.000 

<img width="608" height="149" alt="Screenshot 2026-01-09 211827" src="https://github.com/user-attachments/assets/7efdcf1a-8b2e-475f-a18a-cafe3e96bff1" />

Answer:1482

2.What is the IP associated with the suspected user in the logs?

In the Available field Section there is soure_ip field where the second ip 192.166.65.54 only 0.4% of logs come from. Therefore Browne was the suspect who is associated with 192.166.65.54

<img width="190" height="182" alt="Screenshot 2026-01-09 211946" src="https://github.com/user-attachments/assets/1afaa537-77e2-48a1-950d-cc1b0d847ebb" />

Answer:192.166.65.54

3.The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?

To find out the binary file we add the suspected ip 192.166.65.54 to the filters, We find 2 events. When we click expanded document of that event we see a user_agent → bitsadmin that was the binary file.

<img width="702" height="275" alt="Screenshot 2026-01-09 212257" src="https://github.com/user-attachments/assets/194d74eb-1d71-47be-b66e-c0854e52e500" />
<img width="476" height="350" alt="Screenshot 2026-01-09 212429" src="https://github.com/user-attachments/assets/7b695e4a-535c-45d1-a202-1dc8d2823839" />

Answer: bitsadmin

4.The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?

In the Same Expanded Document we see host field where pastebin.com is the file sharing site for the C2 server.

<img width="479" height="363" alt="Screenshot 2026-01-09 212521" src="https://github.com/user-attachments/assets/0ac6da70-1d38-44ac-b4aa-1220662c4296" />

Answer:pastebin.com 


5.What is the full URL of the C2 to which the infected host is connected?

In that same Expanded Document we see uri field → /yTg0Ah6a. In the question they have asked the full URL path so we add both the site name + the path to get full path of C2.

<img width="473" height="313" alt="Screenshot 2026-01-09 212627" src="https://github.com/user-attachments/assets/9bb0effd-ee2d-43ea-941f-347f126b1129" />

Answer: pastebin.com/yTg0Ah6a


6.A file was accessed on the filesharing site. What is the name of the file accessed?

To know the file name we need to visit pastebin.com/yTg0Ah6a to see the file name. In Top there will be the file name.

<img width="719" height="186" alt="Screenshot 2026-01-09 212750" src="https://github.com/user-attachments/assets/ebb9a952-7ecf-4bf6-940c-afd6a032f35c" />

Answer:secret.txt


7.The file contains a secret code with the format THM{_____}.

The text field contains the flag for the challenge 
<img width="422" height="150" alt="Screenshot 2026-01-09 212830" src="https://github.com/user-attachments/assets/977bdd33-d613-4870-8e7e-64592215c852" />


Answer: THM{SECRET__CODE}

### Done By Ruthran-sec

