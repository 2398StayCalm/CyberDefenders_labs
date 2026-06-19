# 💻 S3CredentialsHunt 
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/s3credentialshunt/]

### 📓 Scenario:
A prominent U.S.-based organization recently received an official notification from AWS alerting them to a potential security breach: one of their AWS access keys appears to have been compromised. Coincidentally, a subsequent internal security alert indicated that CloudTrail logging had been unexpectedly disabled. As a cybersecurity specialist, your task is to thoroughly investigate this series of events, and figure out the timeline of these events.

### ❓Questions❓:

#### ❔  1: To properly track the activities done by the attacker in the environment, you will need to determine the source of the attack. What is the attacker's IP address?

Because I prefer using grep instead of jq, in the first question I focused on unpacking all .json log files into one new directory "LOGS".

<img width="1712" height="77" alt="image" src="https://github.com/user-attachments/assets/51108463-d76c-49ad-a56a-6b619d8d69d8" />
<img width="1585" height="801" alt="image" src="https://github.com/user-attachments/assets/a06a6b04-37ab-4f41-b7c2-a69e8ae48efe" />

Now it's time for grep. First I wanted to check if there is any kind of "ip" in those logs, but the result was too chaotic. I figure why not to try "source". That was a great choice but I needed to get rid of repetitive addresses. I also decided to count uniqe addresses.

<img width="1446" height="152" alt="image" src="https://github.com/user-attachments/assets/2459515b-3397-4d0e-b5a5-1bacf8074bec" />

Now it's not much so I can check all of them. What I discovered seems obvious after usign AbuseIPDB. Only one of them is not from the US, but actually very opposite 😄

It worked! 🔥

#### ❔  2: A timeline for the incident will help identify the gaps in your investigation. When did the attacker interact with the server for the first time?

With the attacker's IP Address identified I decided to grep for some more details related to it. So let's see what 10 lnes back shows for every result with that IP address. An now I can see clearly the how the log is structured. There is a field "eventTime" which shows the date of interactions with our IP. So I'll grep only field ralated to date and it occur that it's already sorted chronologically. The answer should be at the top.

<img width="1147" height="165" alt="image" src="https://github.com/user-attachments/assets/cf2bc86d-bf0b-4921-8dd3-5c7276e8c86d" />

It worked! 🔥

#### ❔  3: To ensure the environment is safe after the recent breach, identifying the attacker's entry point is essential. What is the exact file path from which the attacker retrieved the compromised user access key?

So first I know which file I can grep to find information about access key. If the first file is related to logs with first interaction with the attacker, then somwhere there a can expect the access key to appear. So I grep "access" from that file and expend it a bit to see more details.

<img width="1682" height="599" alt="image" src="https://github.com/user-attachments/assets/e7e7f16e-e73c-49e9-aeed-feadb91a1fb2" />

It worked! 🔥

#### ❔  4: In a cloud environment, determining the user context can help assess the potential extent of damage based on the permissions and resources accessible to that user. What is the 'Type' of the user who disabled CloudTrail?

Here is a little bit tricky. First I tried to correlate the compromised user and some type of Disabling action. Didn't work. So I decided to go back to the malicious IP Address and grep that from logs. There is some action, so let's narrow that to cloudtrail.... and BINGO.

<img width="1902" height="671" alt="image" src="https://github.com/user-attachments/assets/7a9739e0-c9cb-4d62-b98c-c9218438c3c3" />

We can clearly see that this action is about deleting Cloud trail which must be related to disabling action. Let's focus on that. Grep the eventName and expand the result 30 lines back to get more details about the user that run this process. We got the Name of that user and it's access type.

<img width="1482" height="552" alt="image" src="https://github.com/user-attachments/assets/bc30eab0-0409-4362-aab3-f743d50db9d7" />

It worked! 🔥

#### ❔  5: From the previous question, you determined the attacker's access type; now, you need to figure out what resources the attacker has access to. What is the name of the role the attacker takes advantage of to escalate his privilege in the environment?

For this one I did grep the access type from all logs. One of the parameters from userIdentity.

<img width="1397" height="217" alt="image" src="https://github.com/user-attachments/assets/740c687f-8217-42db-80e7-534cbaaa9c55" />

I worked! 🔥

#### ❔  6: In AWS, to enhance security, users are granted temporary access to specific resources when they assume a role. This action results in the creation of a session, characterized by a unique name and credentials. To trace the attacker's TTPs, can you identify the name of the session they initiated?

Same command from the previous question worked here as well. The result show the session name in the principalId parameter.

<img width="1397" height="217" alt="image" src="https://github.com/user-attachments/assets/d527e1c3-e654-4b7d-8191-6e3cee1cbe02" />

It worked! 🔥

#### ❔  7: To effectively mitigate risks, it's crucial to determine whether the attacker attempted to establish persistence for ongoing access. Can you identify the name of the user the attacker created to maintain a foothold on the machine?

As it was with diabling Cloud Trail, here with the creation of a user there should be a request fo that. Assuming that in order to make that request, the attacker used an access type from previous tasks I grep the access type and expand it to catch any kind of requests. Scrolling up the results, there is one request with userName parameter. This looks suspicious because of the actual name that has been created.

<img width="1422" height="74" alt="image" src="https://github.com/user-attachments/assets/3afc84e6-f3f7-4e98-928b-114060e4791b" />
<img width="1880" height="312" alt="image" src="https://github.com/user-attachments/assets/a65ae993-4914-4302-be46-b722ab949b7e" />

It worked! 🔥

#### ❔  8: The attacker leveraged several techniques to access the account they created. What is the event name associated with their initial method?

I did grep the account that was created to see how many times it appears in logs. Not so many.

<img width="1560" height="200" alt="image" src="https://github.com/user-attachments/assets/32442978-7a28-4d2a-80ee-32cf343935e7" />

Now I decided o expand it a little bit and see if there were any Events related to that account, for example LogonEvent.

Based on that search, I can see 4 events ralted to that account, where the EventType is type that logs direct programmatic actions taken within your AWS account. It records interactions made via the AWS Management Console, SDKs, or Command Line Tools. There is a user creation, access key creation for that user, creating login profile (which looks interesting having the question that we have) and another one that will maybe help us in the next question - adding user to a group. Let's focus on the third one where we can clearly see its name - and since that is what I'm looking for...

<img width="1662" height="537" alt="image" src="https://github.com/user-attachments/assets/120cb128-fc3e-451b-8714-530322d729b7" />

It worked! 🔥

#### ❔  9: What is the name of the group the attacker added the newly created user to?

As I've established in the previous task the fourth event was about adding user to a group. Let's take a closer look at this. I foced on that event and expended it 10 lines each way. In the "requestParameter" I can see the answer.

<img width="1901" height="496" alt="image" src="https://github.com/user-attachments/assets/12f964d6-a60f-49df-bfc5-833f3b3545dd" />

It worked! 🔥

#### ❔  10: Sophisticated threat actors often attempt to conceal their TTPs. Can you provide the exact time CloudTrail logging was disabled?

From experience I know that the event ralated to disabling CloudTrail is absed on deleting the trail itself, so I did grep Delete event related to cloudtrail, and expanded it a bit.

<img width="1857" height="497" alt="image" src="https://github.com/user-attachments/assets/e1084606-97b7-4892-a930-07af8d9258cf" />

It worked! 🔥

All Done! 🥇
