# 💻 GoogleCloudHunt 
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/googlecloudhunt/]

### 📓 Scenario:
A wide credential breach has recently affected multiple organizations across various sectors. This incident has raised concerns about potential unauthorized access to your organization operating within the Google Cloud environment. As a cybersecurity analyst, your task is to conduct a thorough investigation to determine the extent of the compromise.

### ❓Questions❓:

#### ❔  1: We need to determine if a user has fallen victim to the credential breach, which user account was compromised?

This one is actually easy to find in the JSON log in the top of the file. It's under "principalEmail" value.

<img width="897" height="452" alt="image" src="https://github.com/user-attachments/assets/6cccea15-14c4-4501-848f-b0cc49c8dd53" />

It worked! 🔥

#### ❔  2: In the attacker's initial exploration of the environment, what is the name of the first Google Cloud Storage bucket they accessed?

From now on I've decided to use grep instead of jq commandline tool, just because I feel more comfortable with that. To do that, first I wanted to find out what is the name of a field containing the bucket name. It turned out that it was bucket_name, so pretty cool. I tried to grep only the filed but there were lots of empty lines, so I adjusted the command and there was the result.

<img width="1287" height="587" alt="image" src="https://github.com/user-attachments/assets/6686d477-cc81-47e5-a2f7-2a4eaca32b02" />

It worked! 🔥

#### ❔  3: Considering the attacker's focus on data exfiltration, what's the name of the object they may have exfiltrated within the first accessed bucket?

Assuming that the object is located in the bucket which I pointed in the previous answer, I decided to grep that bucket and maybe anything it has inside.

<img width="1287" height="66" alt="image" src="https://github.com/user-attachments/assets/fbe9a526-c435-4329-a8a0-218cf068cf77" />

It worked! 🔥

#### ❔  4: Knowing each step the attacker takes is crucial. What's the name of the Compute Engine instance the attacker accessed?

Just by searching for "instance" I did scroll a little bit up and I saw that there is a filed instance_name, so let's try this. It seems like there is only one name that will fit.

<img width="1035" height="47" alt="image" src="https://github.com/user-attachments/assets/357dfd32-3b18-4d9f-bd77-1dca66260148" />

It worked! 🔥

#### ❔  5: What service account is used by the Compute instance for calls to Google Cloud APIs?

After narrowing the filter I've been able to find the exact account that was in the question. I ussed Compute Engine instance from the previous question, then I correlated it to the Google Cloud and I've norrowed do email addresses only.

<img width="1290" height="152" alt="image" src="https://github.com/user-attachments/assets/c5f07d3a-174a-4d69-95f1-ad8a98482a48" />

It worked! 🔥

#### ❔  6: In the process of exfiltrating data, identify the Google Cloud SQL database the attacker attempted to export. What is the database's name?

This one was easy to find. I've decided to grep "sql" from the file and then expand results for 5 more lines. There we can clearly see the answer.

<img width="1050" height="142" alt="image" src="https://github.com/user-attachments/assets/588ad2fc-f11f-466a-b86e-7a7a4726e9f5" />

It worked! 🔥

#### ❔  7: Tracking the data movement, which Google Cloud Storage bucket did the attacker attempt to export the database to?

So for the start I did grep the databas which was mentioned in the last question and expanded search for 10 lines forward to see if there is anything related to export. There was, so next analogically I did the same thing searching export context and check if there was anything related to "backup". Results were satisfying.

<img width="1185" height="66" alt="image" src="https://github.com/user-attachments/assets/908ee973-0eb2-492b-81cd-ac63dff20796" />

It worked! 🔥

#### ❔  8: In the attacker's effort to maintain access, identify the new service account created by the attacker. What is the account ID of this service account?

Similar action with a new account creation. i expect that there is something related with creation in the log, so let's grep "create" and see if there is something.

<img width="1897" height="255" alt="image" src="https://github.com/user-attachments/assets/a5ec0a1e-21ec-4fc5-bbd5-099ec5267d14" />

There is. Now we need an account ID of a service account so let's focus on permission value "iam.serviceAccounts.create" and expand that result for 10 lines back and forward. There it is 😄.

<img width="1252" height="476" alt="image" src="https://github.com/user-attachments/assets/05bbe24c-bd7f-4452-b8e1-1c6e5cfa54c9" />

It worked! 🔥

#### ❔  9: As part of establishing persistence, what is the ID of the secret key generated for the newly created service account?

Now if I know the account ID of that new service account, for sure there is a secret key generated close to it. Lets grep that account ID and look for any sort of "key"

<img width="1902" height="67" alt="image" src="https://github.com/user-attachments/assets/9570c852-40e1-4964-a2cd-ab6bf682aaae" />

It worked! 🔥

All done 🥇
