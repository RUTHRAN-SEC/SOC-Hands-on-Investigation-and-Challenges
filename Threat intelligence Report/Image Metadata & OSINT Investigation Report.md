# Image Metadata & OSINT Investigation – Criminal Location Identification

## Blue Team Online

## Scenario

Law enforcement intercepted publicly posted images shared by a criminal suspect currently evading capture. The images were accompanied by the message: *“I'm roaming free. You will never catch me.”*

As part of the cyber investigation team, you are tasked with conducting forensic and metadata analysis of the images to:

- Extract hidden metadata (EXIF data)
- Identify geolocation coordinates (if present)
- Determine device information used to capture the image
- Analyze timestamps and environmental clues
- Correlate findings with open-source intelligence (OSINT)

The objective is to uncover actionable intelligence that may assist in identifying the suspect’s location or movement patterns.

## Alert

**Suspicious Image Intelligence Alert:** Publicly shared images by a wanted criminal may contain embedded metadata capable of revealing location, device details, or other identifying artifacts. Initial assessment suggests the images could expose geolocation coordinates or traceable digital fingerprints.

A forensic metadata and OSINT analysis is required to extract hidden information and support investigative efforts to locate the suspect.

## Given Files

```jsx
3496 -rw-r--r-- 1 root root 3575684 Nov 26  2021 uploaded_1.JPG
1180 -rw-r--r-- 1 root root 1203827 Nov 26  2021 uploaded_2.png
```

## Tools used

- Exiftool
- Reverse Image Search

---

## Challenge Questions

1.What is the camera model?

To find out the camera model we are using a tool called Exiftool, EXIF (Exchangeable Image File Format) files store important data about photographs.

<img width="960" height="377" alt="Screenshot 2026-03-11 184126" src="https://github.com/user-attachments/assets/e91fb39d-7e87-425e-871b-56f0571290df" />
The camera model is Canon EOS 550D

Answer: Canon EOS 550D

---

2.When was the picture taken? 

To find out the picture taken timestamp detail, we have to find the field Modify Date this shows us the timestamp details.

<img width="960" height="396" alt="Screenshot 2026-03-11 184735" src="https://github.com/user-attachments/assets/e77846dd-5f36-4593-a969-3c09f52c31a1" />
Answer: 2021:11:02 13:20:23

---

3.What does the comment on the first image says? 

The comment detail on the image will be stored as ‘comment’ field 

<img width="960" height="391" alt="Screenshot 2026-03-11 185146" src="https://github.com/user-attachments/assets/b0716d2d-a81f-4457-ab9b-398a7f13a963" />
Answer: relying on altered metadata to catch me?

---

4.Where could the criminal be? 

In the image one uploaded_1.png , the gps details where not accurate or modified the  location detail, it show that the target was in Indian Ocean.

<img width="960" height="220" alt="Screenshot 2026-03-11 185948" src="https://github.com/user-attachments/assets/043aaf4f-9979-4f35-81b1-c0fccf5c0f45" />
To find out i have done OSINT on the image using a google image search, but the uploaded_1.png was not the much clear, so i have used uploaded_2.jpg image.

<img width="905" height="426" alt="Screenshot 2026-03-11 190521" src="https://github.com/user-attachments/assets/7baed459-0d6c-4346-b06c-66b0f729bcb3" />

This was the image in uploaded_2.jpg

The OSINT result

<img width="823" height="341" alt="Screenshot 2026-03-11 190747" src="https://github.com/user-attachments/assets/d70e3839-38a4-4f71-bf06-82b68ede0b52" />

Answer: Kathmandu

---

## Author

### RUTHRAN-SEC
