# 💻 LGDroid 
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/lgdroid/]

### 📓 Scenario:
On May 21, 2021, an intelligence agency intercepted a mobile device suspected of covert operations. The forensic team performed a full disk dump, extracting databases, logs, and application activity. Findings suggest encrypted communications, anonymous browsing, and unauthorized data transfers. Analyze extracted data to determine suspect activities, network connections, and security risks while establishing a timeline of events.

### ❓Questions❓:

#### ❔  1: What is the email address of Zoe Washburne?

To begin the investigation, I examined the extracted contents of the forensic image. As shown in the first screenshot, the extraction produced several artifacts, including the Agent Data directory, which contains multiple SQLite databases relevant to the device's user activity.

<img width="1412" height="402" alt="image" src="https://github.com/user-attachments/assets/673e8cac-f971-4679-a966-ceeb36cc1dbd" />

Since contact information is commonly stored in a contacts database, I navigated to the Agent Data directory and identified contacts3.db. I opened this database using DB Browser for SQLite and browsed the acquired_contacts table.

As shown in the second screenshot, I located the entry for Zoe Washburne.

<img width="1915" height="532" alt="image" src="https://github.com/user-attachments/assets/a92d2e82-8a75-4ec1-bd68-7e0aa6ecb808" />

Answer: zoewash@ox42.null

It worked! 🔥 

#### ❔  2: What was the device's DateTime in UTC at the time of acquisition?

To determine the device's date and time at the moment of acquisition, I examined the Live Data directory generated during the forensic extraction. This directory contains several text files with system information captured from the device.

I displayed the contents of the device_datetime_utc.txt file using the cat command. The file contains the device's recorded UTC date and time at the time of acquisition.

<img width="1142" height="187" alt="image" src="https://github.com/user-attachments/assets/0ac58e60-4fc1-43ff-bd9a-e9587539d8ed" />

Answer: 2021-05-21 18:17

It worked! 🔥

#### ❔  3: What time was the Tor Browser downloaded in UTC?

To determine when Tor Browser was downloaded, I examined the downloads.db SQLite database located in the Agent Data directory. This database contains records of files downloaded to the device.

<img width="1122" height="362" alt="image" src="https://github.com/user-attachments/assets/d9760aac-af95-4c3c-8e59-4134db361d4d" />

As shown, I opened downloads.db in DB Browser for SQLite and reviewed the downloads table. I identified the entry tor-browser-10.0.15-android-armv7-multi.apk. The corresponding lastmod value was 1619725346000, which appears to be a Unix timestamp stored in milliseconds.

<img width="1546" height="497" alt="image" src="https://github.com/user-attachments/assets/b4ef439b-8239-4f70-acb5-7e630035564b" />

I then converted the timestamp using the Epoch Converter tool. As shown in Figure 5, the converted timestamp corresponds to Thursday, April 29, 2021, at 7:42:26 PM UTC.

<img width="1761" height="832" alt="image" src="https://github.com/user-attachments/assets/2833bb90-63b6-4d66-a789-e173605cae0b" />

Answer: 2021-04-29 19:42

It worked! 🔥

#### ❔  4: At what time did the phone reach a 100% charge after the last reset?

To determine when the device reached a full battery charge, I examined the batterystats.txt file located in the Live Data/Dumpsys Data directory. This file contains the Android battery history log, including charging events and battery level changes.

<img width="766" height="612" alt="image" src="https://github.com/user-attachments/assets/73be0b9d-a5a1-4915-83ed-092a22c527b5" />

As shown, I searched the battery history for entries related to a fully charged state. The log begins with the last battery statistics reset at: 2021-05-21 13:12

<img width="941" height="775" alt="image" src="https://github.com/user-attachments/assets/5143b30c-1d6b-4a6f-9f1f-25e03bb43aad" />

The battery history shows the device starting at 99% charge and charging via USB. Further down the log, the entry: +5m01s459ms (3) 100 status=full charge=2665 indicates that the battery reached 100% and entered the full charging state 5 minutes and 1.459 seconds after the reset.

Adding this offset to the reset time:
Reset time: 2021-05-21 13:12
Offset: +00:05
Results in: 2021-05-21 13:17 UTC (approximately).

<img width="767" height="392" alt="image" src="https://github.com/user-attachments/assets/21722ada-f756-435b-a43f-dffdd1a7261f" />

Answer: 2021-05-21 13:17

It worked! 🔥

#### ❔  5: What is the password for the most recently connected WIFI access point?

To identify the password of the most recently connected Wi-Fi network, I examined the Android system data extracted from adb-data.tar. After navigating to the ADB-DATA/apps/com.android.providers.settings/ directory, I located the com.android.providers.settings.data file, which contains device configuration information, including saved wireless network profiles.

<img width="897" height="241" alt="image" src="https://github.com/user-attachments/assets/62416052-20be-4e1a-9781-669329c0d9cd" />

As shown, the file contains XML-formatted Wi-Fi configuration data under the <WifiBackupData> section. Within the network profile, I identified the SSID Hot_SSL and the security configuration WPA_PSK. The Wi-Fi password is stored in the PreSharedKey field, which represents the WPA/WPA2 pre-shared key (PSK) used to authenticate to the wireless network.

The entry: <string name="PreSharedKey">ThinkingForest!</string> reveals the password associated with the saved Wi-Fi network.

<img width="1267" height="765" alt="image" src="https://github.com/user-attachments/assets/8a84ee3a-3cd0-47e4-b158-7a2dbeed3e65" />

Answer: ThinkingForest!

It worked! 🔥

#### ❔  6: What app was the user focused on at 2021-05-20 14:13:27?

To determine which application the user was actively using at the specified time, I examined the appops.txt file located in the Live Data/Dumpsys Data directory. This artifact contains Android AppOps records, including application permissions and timestamps indicating when applications accessed specific resources.

<img width="1802" height="352" alt="image" src="https://github.com/user-attachments/assets/a51c1d58-62b6-4496-b14b-8bf74a02bc52" />

As shown, I searched the file for the timestamp 2021-05-20 14:13:27 using the grep command. The search returned an entry associated with the package com.google.android.youtube. The timestamp appears in the application's permission activity records, indicating that YouTube was the application in focus at that time.

<img width="1060" height="515" alt="image" src="https://github.com/user-attachments/assets/a989fb1f-8832-49ab-aa38-073c63d49462" />

Answer: com.google.android.youtube (YouTube)

It worked! 🔥

#### ❔  7: How long did the suspect watch YouTube on 2021-05-20?

To determine how long the suspect used YouTube, I examined the usagestats.txt file located in the Live Data/Dumpsys Data directory. This artifact contains Android application usage statistics, including application launch counts, last usage times, and total time spent in each application.

<img width="1781" height="272" alt="image" src="https://github.com/user-attachments/assets/2b5f263d-4b9d-402c-a9a1-bcf5d6b651ef" />

As shown, I searched the file for entries related to YouTube using the package name com.google.android.youtube. The results revealed a usage statistics record containing the field: totalTime="8-34-29"

<img width="1722" height="480" alt="image" src="https://github.com/user-attachments/assets/338dfef9-769a-4564-a094-6639c82ad61f" />

This value represents the cumulative amount of time the application was actively used on the device. The associated entry also shows a last usage timestamp of 2021-05-20 22:47, confirming activity on the date under investigation.

Answer: 8 hours, 34 minutes

#### ❔  8: What is the structural similarity metric for the image "suspicious.jpg" compared to a visually similar image taken with a mobile phone?

To analyze the image, I located suspicious.jpg in the extracted forensic data. The file was then uploaded to the Alma Technologies Online Comparison of Image Compression Standards tool, which calculates image quality metrics, including the Structural Similarity Index (SSIM).

<img width="1052" height="560" alt="image" src="https://github.com/user-attachments/assets/66adb66a-cd8e-4ed6-9ded-af151563155b" />
<img width="1282" height="737" alt="image" src="https://github.com/user-attachments/assets/0b138144-826a-44af-bff5-6acd9d83ae96" />

As shown, the tool processed the image and returned several compression comparison results. The SSIM value ranges from 0 to 1, where values closer to 1 indicate a higher degree of similarity between images. A score of 0.9873 indicates that the image is extremely similar to the comparison image, with only minor differences.

I analyzed the similar image taken with a mobile phone and results are presented below.

<img width="976" height="377" alt="image" src="https://github.com/user-attachments/assets/b76f046b-8988-46f3-8a2c-fee60e3b78bd" />

The answer: 0.99

It worked! 🔥
