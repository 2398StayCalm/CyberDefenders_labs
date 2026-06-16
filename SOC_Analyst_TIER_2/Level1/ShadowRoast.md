# 💻 ShadowRoast
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/shadowroast/]

### 📓 Scenario:
As a cybersecurity analyst at TechSecure Corp, you have been alerted to unusual activities within the company's Active Directory environment. Initial reports suggest unauthorized access and possible privilege escalation attempts.

Your task is to analyze the provided logs to uncover the attack's extent and identify the malicious actions taken by the attacker. Your investigation will be crucial in mitigating the threat and securing the network.

### ❓Questions❓:

#### ❔  1: What's the malicious file name utilized by the attacker for initial access?

After a lot of obvious missed searches, I've decided to search all events that were related to "Downloads" directory. It found 2 event in which a CommandLine was using the malicious file that we're looking for.

<img width="977" height="552" alt="image" src="https://github.com/user-attachments/assets/3f35cf98-defe-46fb-8209-8efc20c1ce1f" />

It worked! 🔥

#### ❔  2: What's the registry run key name created by the attacker for maintaining persistence?

Now we can correlate the malicious file name with a "registry" keyword. It will return 1 event which contains the Target Object with the registry run key name.

<img width="1080" height="257" alt="image" src="https://github.com/user-attachments/assets/b93d5053-049d-4719-8a14-9814619ff70a" />

It worked! 🔥

#### ❔  3: What's the full path of the directory used by the attacker for storing his dropped tools?

Very often attackers use a directory for tools/logs/files and all the stuff related to the attack that is temporary. So what I did was searching for a "temp" folder correlated with a user that had the malicious file on his machine.

<img width="1247" height="171" alt="image" src="https://github.com/user-attachments/assets/616ab657-87db-44b8-9397-1aa88356cf98" />

Next I've checked "winlog.event_data.OriginalFileName" values and I've recognized one popular tool Rubeus.exe, So I've checked the CurrentDirectory of that file from fields that are available.

It worked! 🔥

#### ❔  4: What tool was used by the attacker for privilege escalation and credential harvesting?

The answer was already in the last question 😃

It worked! 🔥

#### ❔  5: Was the attacker's credential harvesting successful? If so, can you provide the compromised domain account username?

To narrow the searching scope, I've decided to look if there were any files dumped with some data to the same directory using the attackers tools. I found out that there is more well-known tools available, for example Mimikatz. What got my attention was field "winlog.event_data.User" and I spotted that there was more than one user available. I believe that NT Authority wasn't the case so I went for the other one.

<img width="1072" height="156" alt="image" src="https://github.com/user-attachments/assets/12c516ae-dc4c-4bcd-8f0c-e4d75df706c3" />

It worked! 🔥

#### ❔  6: What's the tool used by the attacker for registering a rogue Domain Controller to manipulate Active Directory data?

For that, actually, was responsible the other tool that I found and wrote in the last question.

It worked! 🔥

#### ❔  7: What's the first command used by the attacker for enabling RDP on remote machines for lateral movement?

First I want all events that contain CommandLine field so let's filter that. Now I know that there was a successful attempt to harvest credential of one user so let's add him to filters. Now it gave me 18 results when I parse it to the table view. There is only 1 command that modifies Windows Registry setting. It adds value including registry key containing Remote Desktop configuration settings. it also forces overwrite without prompting for confirmation so that's a strong clue.

<img width="1267" height="566" alt="image" src="https://github.com/user-attachments/assets/e09c0b2f-7d98-4bad-bb6e-1d22dda6b0fa" />

It worked! 🔥

#### ❔  8: What's the file name created by the attacker after compressing confidential files?

In the search bar I left only a user that was compromised and I've decided to shoot for any ZIP files related to that user. The result was perfect.

<img width="1152" height="622" alt="image" src="https://github.com/user-attachments/assets/c3ef9836-9833-4152-911d-a7d49ecf9021" />

It worked! 🔥

All Done! 🥇
