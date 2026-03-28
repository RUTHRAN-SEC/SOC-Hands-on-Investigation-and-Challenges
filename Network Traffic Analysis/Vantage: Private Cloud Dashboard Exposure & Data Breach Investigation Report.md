# Vantage: Private Cloud Dashboard Exposure & Data Breach Investigation Report

## HackTheBox

## Scenario

A small organization recently migrated part of its infrastructure to a private cloud environment. During deployment, developers unintentionally left an exposed redirect to the cloud management dashboard accessible via the public-facing web server.

Shortly after the migration, the security team received an email from an individual claiming to have accessed internal systems and exfiltrated sensitive user data.

As the assigned analyst, your objectives are to:

- Investigate the exposed web server redirect
- Determine whether unauthorized access occurred
- Validate the attacker’s data leak claims
- Identify the scope of compromised resources
- Assess whether user data was accessed or exfiltrated
- Provide remediation recommendations

The goal is to determine whether this is a legitimate breach or a false claim.

## Alert

**Security Notification: Alleged Data Breach & Unauthorized Cloud Access**

The security team received a direct communication from an external actor claiming to have gained access to the organization’s private cloud dashboard and leaked user data.

Preliminary concerns include:

- Publicly accessible redirect to internal cloud dashboard
- Potential authentication bypass or weak access controls
- Possible unauthorized data access or exfiltration
- Risk to customer confidentiality and regulatory compliance

Severity: **High (Pending Validation)**

## Tools Used:

- WireShark

## Given Files

```jsx
    8 -rw-r--r-- 1 root root     6148 Jul  2  2025 .DS_Store
 7004 -rw-r--r-- 1 root root  7170127 Jul  1  2025 controller.2025-07-01.pcap
14748 -rw-r--r-- 1 root root 15100111 Jul  1  2025 web-server.2025-07-01.pcap
```

## Challenge Questions

1.What tool did the attacker use to fuzz the web server ? (Format- include version)

We analysis the web-server.pcap first cause the fuzz tools are used to enumerate the site, so i have Analyzed the pcap on http filter.

On the packet 

```jsx
 116	2025-07-01 09:38:26.475789 117.200.21.26 157.230.81.229	HTTP	180	GET / HTTP/1.1 
```

<img width="469" height="126" alt="Screenshot 2026-03-28 130012" src="https://github.com/user-attachments/assets/c2c3a3ad-ee72-4326-88c1-9e17f40c37e4" />

The tool used by the attacker was FFUF v2.1.0 on 2025-07-01 09:38:26 to enumerate subdomain of the site.

Answer: FFUF v2.1.0

---

2.Which subdomain did the attacker discover?

The filter used:

```jsx
http.user_agent contains "Fuzz" 
```

We know the attacker is using the FFUF tool so i filtered it out with the user agent.

On the packet:

```jsx
2373	2025-07-01 09:38:37.465869	10.116.0.3	10.116.0.4	HTTP	316	GET / HTTP/1.1 
```

The attacker found a cloud subdomain using the FFUF tool and redirected to the site. 

<img width="633" height="409" alt="Screenshot 2026-03-28 131503" src="https://github.com/user-attachments/assets/ff444ae8-8125-47b2-bbbf-14c8777d9343" />

Answer: cloud

---

3.How many login attempts did the attacker make before successfully logging in to the dashboard?

The filter Used:

```jsx
ip.addr == 117.200.21.26 && http.request.uri contains "login"
```

Used the attacker IP and using the word “login” and filtering the http request 

<img width="956" height="103" alt="Screenshot 2026-03-28 133745" src="https://github.com/user-attachments/assets/c1cc9fda-8515-415f-ae36-151e048eb861" />

The attacker made 3 login attempt and one successful login. 

Answer: 3

---

4.When did the attacker download the OpenStack API remote access config file? (UTC)

The filter used 

```jsx
http.request.method == "GET" and ip.addr==117.200.21.26
```

changed length into descending order

On the packet

```jsx
21247	2025-07-01 09:40:29.954083	117.200.21.26	157.230.81.229	HTTP	769	GET /dashboard/project/api_access/openrc/ HTTP/1.1 
```

<img width="960" height="339" alt="Screenshot 2026-03-28 135350" src="https://github.com/user-attachments/assets/41b7717b-b13c-4db3-8315-a7a64a6e5cfa" />

Answer: 2025-07-01 09:40:29

---

5.When did the attacker first interact with the API on controller node? (UTC)

The Filter used on the file controller.pcap 

```jsx
ip.src == 117.200.21.26 && http
```

the first request from the attacker.

```jsx
8490	2025-07-01 09:41:44.667723	117.200.21.26	134.209.71.220	HTTP	293	GET /identity HTTP/1.1 
```

Answer: 2025-07-01 09:41:44

---

6.What is the project id of the default project accessed by the attacker?

The Filter used on the file controller.pcap 

```jsx
ip.src == 117.200.21.26 && http && frame contains "projects"
```

On the packet 

```jsx
23129	2025-07-01 09:49:15.122075	117.200.21.26	134.209.71.220	HTTP	621	PUT /identity/v3/projects/9fb84977ff7c4a0baf0d5dbb57e235c7/users/c373da67a62b48f393c45dc071fa80b8/roles/0501401642464242bcd799437b71bdc9 HTTP/1.1 
```

<img width="954" height="198" alt="Screenshot 2026-03-28 151156" src="https://github.com/user-attachments/assets/9abf1783-949e-4362-b729-93d35447a981" />

<img width="635" height="412" alt="Screenshot 2026-03-28 151341" src="https://github.com/user-attachments/assets/27835e6a-6b3f-4773-865b-f85adb3e1554" />

Answer: 9fb84977ff7c4a0baf0d5dbb57e235c7

---

7.Which OpenStack service provides authentication and authorization for the OpenStack API?

The Filter used on the file controller.pcap 

```jsx
ip.src == 117.200.21.26 && http 
```

On the packet 

```jsx
8506	2025-07-01 09:41:45.271347	117.200.21.26	134.209.71.220	HTTP/JSON	298	POST /identity/v3/auth/tokens HTTP/1.1 , JavaScript Object Notation (application/json)
```

<img width="478" height="162" alt="Screenshot 2026-03-28 190953" src="https://github.com/user-attachments/assets/796e5fd5-bf14-4298-970d-b8896597031e" />

Answer: keystone

---

8.What is the endpoint URL of the swift service?

The Filter used on the file controller.pcap 

```jsx
ip.src == 117.200.21.26 && http 
```

On the packet

```jsx
12892	2025-07-01 09:43:27.279520	117.200.21.26	134.209.71.220	HTTP	528	GET /v1/AUTH_9fb84977ff7c4a0baf0d5dbb57e235c7?format=json HTTP/1.1 
```

<img width="609" height="157" alt="Screenshot 2026-03-28 192034" src="https://github.com/user-attachments/assets/6ce39549-14a0-4aa0-a358-f4c5174f6079" />

Answer: hxxp[://]134[.]209[.]71[.]220:8080/v1/AUTH_9fb84977ff7c4a0baf0d5dbb57e235c7

---

9.How many containers were discovered by the attacker?

The Filter used on the file controller.pcap 

```jsx
ip.src == 117.200.21.26 && http.request.method == "GET" && http.request.uri contains "/v1/AUTH_"
```

On the packet

```jsx
12892	2025-07-01 09:43:27.279520	117.200.21.26	134.209.71.220	HTTP	528	GET /v1/AUTH_9fb84977ff7c4a0baf0d5dbb57e235c7?format=json HTTP/1.1 
```

<img width="632" height="412" alt="Screenshot 2026-03-28 193114" src="https://github.com/user-attachments/assets/e380e216-9857-4fd0-930d-5aea101b5e03" />

Answer: 3

---

10.When did the attacker download the sensitive user data file? (UTC)

The filter used and changed length into descending order.

```jsx
http.request.method=="GET"
```

On the packet

```jsx
16398	2025-07-01 09:45:23.060797	117.200.21.26	134.209.71.220	HTTP	543	GET /v1/AUTH_9fb84977ff7c4a0baf0d5dbb57e235c7/user-data/user-details.csv HTTP/1.1 
```

<img width="954" height="359" alt="Screenshot 2026-03-28 195156" src="https://github.com/user-attachments/assets/d625cf81-9077-4ea0-9960-33517db928c4" />

Downloading the sensitive data.

Answer: 2025-07-01 09:45:23

---

11.How many user records are in the sensitive user data file?

The filter used and changed length into descending order.

```jsx
http.request.method=="GET"
```

On the packet

```jsx
16398	2025-07-01 09:45:23.060797	117.200.21.26	134.209.71.220	HTTP	543	GET /v1/AUTH_9fb84977ff7c4a0baf0d5dbb57e235c7/user-data/user-details.csv HTTP/1.1 
```

<img width="634" height="411" alt="Screenshot 2026-03-28 200735" src="https://github.com/user-attachments/assets/e0eaa672-b1d2-48b1-a050-4a7c2f18a9de" />

There are totally 28 user sensitive data.

Answer: 28

---

12.For persistence, the attacker created a new user with admin privileges. What is the username of the new user? 

The filter used and sorted the packet number.

```jsx
ip.src == 117.200.21.26 and http
```

On the packet 

```jsx
22774	2025-07-01 09:49:12.653825	117.200.21.26	134.209.71.220	HTTP	516	GET /identity/v3/users?name=jellibean HTTP/1.1 
```

<img width="960" height="191" alt="Screenshot 2026-03-28 202434" src="https://github.com/user-attachments/assets/4ed89c2f-552a-4ffa-b546-7948611af3cb" />

The attacker searched for the user jellibean, When attacker found not there is no user like jellibean in packet number 22694. He created one one 20776.

<img width="629" height="417" alt="Screenshot 2026-03-28 202721" src="https://github.com/user-attachments/assets/9eb811e8-ab97-44ca-af3c-1274b1cb4f53" />

Answer: jellibean

---

13.What is the password of the new user?

The filter used:

```jsx
ip.src == 117.200.21.26 && http.request.method == "POST" && http.request.uri contains "/identity/v3/users"
```

On the packet

```jsx
20776	2025-07-01 09:48:02.472070	117.200.21.26	134.209.71.220	HTTP/JSON	202	POST /identity/v3/users HTTP/1.1 , JavaScript Object Notation (application/json)
```

<img width="629" height="417" alt="Screenshot 2026-03-28 202721" src="https://github.com/user-attachments/assets/896abe66-562e-403e-ab61-990f0354d02d" />

Answer: P@$$word

---

14.What is MITRE tactic id of the technique in task 12?

<img width="677" height="404" alt="Screenshot 2026-03-28 204810" src="https://github.com/user-attachments/assets/b99b65ac-119d-4f31-a7d6-29aaed3a8abe" />

Answer: T1136.003

---

<img width="601" height="225" alt="Screenshot 2026-03-28 205100" src="https://github.com/user-attachments/assets/23f36923-4322-4d3e-8db5-5f1db8e9ca7c" />

## Author

### RUTHRAN-SEC
