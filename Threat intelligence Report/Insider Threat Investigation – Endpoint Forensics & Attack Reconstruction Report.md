# Insider Threat Investigation – Endpoint Forensics & Attack Reconstruction

## Cyber Defender

## Scenario

A client organization experienced a significant security breach that resulted in critical systems being taken offline. Preliminary investigation by incident responders suggests the compromise originated from a single internal user account, raising concerns of a potential insider threat.

Digital forensic artifacts and endpoint evidence have been collected for analysis.

As the assigned investigator, your objectives are to:

- Analyze endpoint evidence and user activity
- Identify the insider responsible for the compromise
- Reconstruct attacker actions and timeline
- Determine how the compromise occurred
- Identify unauthorized access or malicious actions
- Assess the impact of the incident on the environment

The goal is to uncover the attacker’s identity, understand the scope of malicious activity, and support remediation efforts.

## Alert

**Security Incident Alert: Potential Insider Threat Activity Detected**

A major security incident has resulted in disruption of internal systems and forced portions of the network offline. Preliminary forensic analysis indicates that the activity may have originated from a legitimate internal user account.

Observed indicators include:

- Unauthorized or suspicious user activity
- Access to sensitive systems outside normal behavior patterns
- Potential misuse of privileged access
- Actions consistent with deliberate internal compromise

Immediate forensic investigation is required to identify the responsible individual and reconstruct the sequence of malicious activity.

## Tools used

- Google Search Engine (Dorking)

## Given Files

```jsx
   4 -rw-r--r-- 1 root root      31 Jul 27  2021 Github.txt -> https://github.com/EMarseille99 
1032 -rw-r--r-- 1 root root 1055214 Jul 27  2021 WebCam.png
 148 -rw-r--r-- 1 root root  147635 Jul 27  2021 office.jpg
```

## Challenge Questions

1.File -> Github.txt: What API key did the insider add to his GitHub repositories?

We can perform OSINT on the GitHub username “EMarseille99”, I have used google search engine for google dorking. 

#### Search : EMarseille99 "api key"site:github.com

On the repository name Project-Build---Custom-Login-Page was created on May 24, 2020, On the folder name Login Page.js the api key was been exposed. 

The Repo link: https://github.com/EMarseille99/Project-Build---Custom-Login-Page/blob/master/Login%20Page.js

<img width="448" height="311" alt="Screenshot 2026-05-07 194914" src="https://github.com/user-attachments/assets/9d3c8ee0-94ae-45aa-8d43-2f492c47b0fc" />


```jsx
API Key = aJFRaLHjMXvYZgLPwiJkroYLGRkNBW
```

Answer: aJFRaLHjMXvYZgLPwiJkroYLGRkNBW

---

2.File -> Github.txt: What plaintext password did the insider add to his GitHub repositories?

#### Search: EMarseille99 "password"site:github.com

Refer Link: https://github.com/1d8/ctf/blob/main/solutions/cyberdefenders_l_espion.md

<img width="526" height="82" alt="Screenshot 2026-05-07 195705" src="https://github.com/user-attachments/assets/2fecdbf9-91f0-4621-92b1-6b60f54efe05" />

Answer: PicassoBaguette99 

---

3.File -> Github.txt: What cryptocurrency mining tool did the insider use?

#### Search: EMarseille99 "cryptocurrency"site:github.com

<img width="580" height="153" alt="Screenshot 2026-05-07 200023" src="https://github.com/user-attachments/assets/2cd63be1-b2df-4141-b378-7d054f99c448" />

Answer: xmrig

---

4.On which gaming website did the insider have an account?

#### Search: "EMarseille99”

<img width="382" height="80" alt="Screenshot 2026-05-07 200222" src="https://github.com/user-attachments/assets/faf21e13-45a5-4232-a2b1-97d066e782ac" />

Answer: Steam

---

5. What is the link to the insider Instagram profile?

#### Search: "EMarseille99”

Link: https://www.instagram.com/emarseille99/

<img width="557" height="338" alt="Screenshot 2026-05-07 200442" src="https://github.com/user-attachments/assets/139c7dc2-21d4-4d9f-93e1-dd3b091116c3" />

Answer: https://www.instagram.com/emarseille99/

---

6.Which country did the insider visit on her holiday?
Found a post picture form the Instagram and performed OSINT by google len

#### The Post caption: Once in a lifetime holiday here, love me some slings x

The post found:

<img width="293" height="269" alt="Screenshot 2026-05-07 200632" src="https://github.com/user-attachments/assets/bb98f132-e96b-4ae3-80ce-830c91d55038" />

#### The Google len result

This is an image of the iconic **Marina Bay Sands** hotel in Singapore.
• **Structure:** It features three 55-story hotel towers topped by the **Sands SkyPark**, a ship-shaped platform suspended 200 meters above the ground.
• **Amenities:** The SkyPark houses the world's largest rooftop infinity pool.
• **Location:** The complex is located in Singapore's downtown core, fronting Marina Bay. 

Answer: Singapore

---

7. Which city does the insider family live in?
Found a post picture form the Instagram and performed OSINT by google len

#### Post Caption: Nice to meet friends & family Photo 1/2

<img width="291" height="187" alt="Screenshot 2026-05-07 201126" src="https://github.com/user-attachments/assets/70ac42d2-cb21-4d0a-82a7-e17e01d207df" />

#### The Google len result

The image appears to show a residential villa located in **Dubai**, United Arab Emirates. Specific visual cues that place this scene in a UAE city include:

• **National Flag:** The United Arab Emirates flag is clearly visible on the right side of the building.

• **Architecture:** The building features architectural elements typical of residential villas in the UAE, such as arched windows, ornate balconies, and a walled perimeter with decorative iron gates.

• **Vehicles:** The presence of a white Kia Sorento and other modern sedans is consistent with common vehicle types in urban residential areas like **Dubai** or **Abu Dhabi**

Answer: Dubai

---

8.File -> office.jpg: You have been provided with a picture of the building in which the company has an office. Which city is the company located in?

#### The Given Office image

<img width="511" height="273" alt="Screenshot 2026-05-07 201735" src="https://github.com/user-attachments/assets/2b47261a-1cfd-465d-b497-721aba61f08d" />

#### The Google len result

This location is in **Birmingham**, United Kingdom. The image shows a pedestrian signpost in front of the **Birmingham New Street** railway station and the **Grand Central** shopping complex. The signs point toward several of the city's key landmarks, including:

• **Bullring / Markets:** A major retail and historic market area.

• **Birmingham Hippodrome:** A prominent theatre located in the nearby Chinese Quarter.

• **Thinktank:** The city's science museum, located within the Millennium Point complex.

Answer: **Birmingham**

---

9.File -> Webcam.png: With the intel, you have provided, our ground surveillance unit is now overlooking the person of interest suspected address. They saw them leaving their apartment and followed them to the airport. Their plane took off and landed in another country. Our intelligence team spotted the target with this IP camera. Which state is this camera in?

#### The Given Webcam.png image:

<img width="442" height="292" alt="Screenshot 2026-05-07 201951" src="https://github.com/user-attachments/assets/84e64867-994f-44c5-9c4c-fe8ff6ab846a" />

#### The Google len result

• This is a live webcam view located atop the **Main Building** at the University of Notre Dame in South Bend, Indiana.

• The camera offers a bird's-eye perspective of the university's central quadrangle, known as **God Quad**.

• Visible in the scene are prominent campus landmarks, including the **Basilica of the Sacred Heart** and, partially in the distance, **Notre Dame Stadium**.

• Installed in 2015 by EarthCam, this 24-hour feed allows alumni and visitors to view the campus in real-time

Answer: Indiana

---

## Author

### RUTHRAN-SEC
