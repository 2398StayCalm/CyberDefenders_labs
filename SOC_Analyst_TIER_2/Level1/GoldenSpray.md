# 💻 GoldenSpray
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/goldenspray/]

### 📓 Scenario:
As a cybersecurity analyst at SecureTech Industries, you've been alerted to unusual login attempts and unauthorized access within the company's network. Initial indicators suggest a potential brute-force attack on user accounts. Your mission is to analyze the provided log data to trace the attack's progression, determine the scope of the breach, and the attacker's TTPs.


### ❓Questions❓:

#### ❔  1: What is the attacker's IP address?

For the start, we see that in Scenario they said about unusual login attempts and unauthorized access within the network. That means there will be some failed logon requests. So first we can filter logs for Logon events and then check failures with the right Event Code.

<img width="1900" height="302" alt="image" src="https://github.com/user-attachments/assets/56134baf-aab4-472e-a4b1-eca64bef0ead" />

There we have 22 events from which we need to find the attacker's IP address. Let's look winlog.event_data.IpAddress fields. There we can see actually 3 IP Addresses but only one suggest some suspicious source.

<img width="607" height="327" alt="image" src="https://github.com/user-attachments/assets/65509989-89aa-4edb-99aa-b0c21afb8d01" />

It worked! 🔥

#### ❔  2: What country is the attack originating from?

To get that information we can use a crowdsourced threat intelligence platform AbuseIPDB.

<img width="852" height="722" alt="image" src="https://github.com/user-attachments/assets/ecfc7a57-dfc9-466d-95c1-d7413dc7a391" />

It worked! 🔥

#### ❔  3: What's the compromised account username used for initial access?

As we established attacker's IP Address, now we filter all events correlated with it. There we check Event Codes and we see failures, successful logons and logon attempt using explicit credentials. To narrow this one we'll select the last one which is 4648. Now we know that it is a normal user that is compromised so we'll check a computer name that suggest Windows machine from what we have.

<img width="1367" height="845" alt="image" src="https://github.com/user-attachments/assets/f6f69fb7-a0c5-463c-9253-d681654ae4fd" />

It worked! 🔥

#### ❔  4: What's the name of the malicious file utilized by the attacker for persistence on ST-WIN02?

Now we have a compromised username so let's filter for events correlated with that user. We can check what kind of Event Codes are related to that user. One of them is interesting: 13, so let's filter that. The event code refers to Registry values which could point to persistence methods. If we dig deeper we can check RuleName that shows "T1547.001,technique_name=Registry Run Keys / Start Folder" which stronlgy suggest persistence method. Now if we check the target object there we can see suspicious process name. If we dig deeper we can see that it's a file that has a strange file path.

<img width="605" height="251" alt="image" src="https://github.com/user-attachments/assets/444167ce-003b-491a-aba7-00143a7c06f2" />

It worked! 🔥

#### ❔  5: What is the complete path used by the attacker to store their tools?

We can filter for logs related with our compromised user again, and then dig deeper into TargetFileName. There we can see some well known tools like PsExec or mimikatz, and where they're stored.

<img width="1847" height="760" alt="image" src="https://github.com/user-attachments/assets/bb96328a-1f69-4fd2-a6bb-4fa61cac1501" />

It worked! 🔥

#### ❔  6: What's the process ID of the tool responsible for dumping credentials on ST-WIN02?

So what we know is that there are some tools stored and one of them is Mimikatz which is used for credential dumping. Another tip is that credential dumping is usually correlated with lsass process. Now if we search for these two key phrases we'll get our answer which can be read from the log.

<img width="1011" height="387" alt="image" src="https://github.com/user-attachments/assets/4ec9cd55-8b40-49b6-87a3-d37868b78b21" />

It worked! 🔥

#### ❔  7: What's the second account username the attacker compromised and used for lateral movement?

If we go back to the third question, we search there for a first username that was compromised, but there were two users that fitted to the IP Address and the event code.

<img width="590" height="270" alt="image" src="https://github.com/user-attachments/assets/521f2479-ca89-4347-a39f-a6dca5127b23" />

It worked! 🔥

#### ❔  8: Can you provide the scheduled task created by the attacker for persistence on the domain controller?

We search for all unique Evend Codes and then we can check if there are any Codes related to Task Scheduler. We found Event Code 106. It's responsible for creating tasks. Then we know that the task was created for a Domain Controller, so we select only ST-DC01. We're left with 3 different events, but two of them are initiated by Administrator and the third one is unknown.

<img width="956" height="312" alt="image" src="https://github.com/user-attachments/assets/b823fe78-d1c9-411a-914b-89456fbcead6" />

It worked! 🔥

#### ❔  9: What type of encryption is used for Kerberos tickets in the environment?

Here we search for events related to Kerberos, so we used that keyword to filter. Now we can check if any logs have some sort of EncryptionType field. We got all events with that field and the message of the first log contains information about encryption types. All of it is defined in RFC 4120 so we've done a little research using browser. We know that it has to have some weaknesses so it narrows the scope.

<img width="672" height="257" alt="image" src="https://github.com/user-attachments/assets/99b618df-6435-40f4-ae21-1a77732dcd50" />

It worked! 🔥

#### ❔  10: Can you provide the full path of the output file in preparation for data exfiltration?

For this we search for Event Code 11, which points to file creation. Second thing that we check is Rule Names that occured and we see 3 events correlated with a technique "File System Permissions Weakness". We check it and we can see Target File Names related to Location for backup tools of the attacker and also with a ZIP file that could be our target.

<img width="1007" height="251" alt="image" src="https://github.com/user-attachments/assets/fb398b71-dd17-4d3f-baf7-e6bb0747b97c" />

It worked! 🔥

All Done! 🥇
