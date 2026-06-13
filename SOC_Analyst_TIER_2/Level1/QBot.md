# 💻 QBot
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/qbot/]

### 📓 Scenario:
A company's security team detected unusual network activity linked to a potential malware infection. As a forensic analyst, your mission is to investigate a memory dump, identify the malicious process, extract artifacts, and uncover Command and Control (C2) communications. Using Volatility3, analyze the attack, trace its origin, and provide actionable intelligence.


### ❓Questions❓:

#### ❔  1: Our first step is identifying the initial point of contact the malware made with an external server. Can you specify the first IP address the malware attempted to communicate with?

To get that we read already acomplished scans of network connections, and we grep those with TCPv4 protocol which are CLOSED or ESTABLISHED, because we don't know if the connection was successful or not. Usually the attacker protocol with unencrypted traffic. From the results we can see that there is protocol 80. The question is about external server, we can try out all of them.

<img width="1902" height="630" alt="image" src="https://github.com/user-attachments/assets/d3c215df-2427-41ce-8555-3636a70993f6" />

It worked! 🔥

#### ❔  2: We need to determine if the malware attempted to communicate with another IP. Which IP address did the malware attempt to communicate with again?

Same as with the first question, the second IP Address is among those we filtered.

It worked! 🔥

#### ❔  3: Identifying the process responsible for this suspicious behavior helps reconstruct the sequence of events leading to the execution of the malware and its source. What is the name of the process that initiated the malware?

After running window.psscan there is not really suspicious processes that can be recognized at first look. When running windows.pstree we can spot a suspicious situation with one of processes. A default process tree for that program should be different but it also could be configured to run this way (at start of the system). 

<img width="1441" height="186" alt="image" src="https://github.com/user-attachments/assets/625f8294-eeba-41f1-9030-4f013bd38807" />

Nevertheless we should check if that process does something more after being run. Let's check commands related to that process. And there we go, it starts in a waiting state for communication via the Dynamic Data Exchange (DDE) protocol. This is a legacy Windows technology that alows applications to send data and commands.

<img width="1770" height="57" alt="image" src="https://github.com/user-attachments/assets/0385eef7-b4ce-4a49-a507-d8f98cd28146" />

It worked! 🔥

#### ❔  4: The malware's file name is crucial for further forensic analysis and extracting the malware. Can you provide its file name?

For this task we tried windows.dumpfiles for the process that was in the answer from the last question. Nothing interesting was dumped but we noticed that there were some problem with dumping a lot of files. We can filter for those with errors and check if it'll show anything interesting. There was one that got our attention.

<img width="1875" height="582" alt="image" src="https://github.com/user-attachments/assets/6e820055-5967-4652-b7f3-39ae647d5718" />

It worked! 🔥

#### ❔  5: Hashes are like digital fingerprints for files. Once the hash is known, it can be used to scan other systems within the network to identify if the same malicious file exists elsewhere. What is the SHA256 hash of the malware?

To get SHA256 HASH of that file we would have to dump it first but this one gives an Error. Let's check check if this file is edited or maybe damaged and if there is any copy of it in the memory. We have a TXT file that shows scan of files in Artifacts. As we can see there are 2 files with the same name but different Virtual Address. Now we need to dump the other one and get the hash of it with sha256sum command.

<img width="1217" height="113" alt="image" src="https://github.com/user-attachments/assets/1b05dc2d-f411-4076-b5f8-343b125c4077" />

It worked! 🔥

#### ❔  6: To trace the origin of the malware and understand its development timeline, can you provide the UTC creation time of the malware file?

Here we just take the SHA256 hash and check it on VirusTotal.

<img width="697" height="165" alt="image" src="https://github.com/user-attachments/assets/f14d89ca-cab2-4021-95a7-22e2618f6933" />

It worked! 🔥

All Done! 🥇
