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

In the second task I had to use jq CommandLine tool to filter for storage buckets.  IN PROGRESS......

#### ❔  3: Considering the attacker's focus on data exfiltration, what's the name of the object they may have exfiltrated within the first accessed bucket?



#### ❔  4: Knowing each step the attacker takes is crucial. What's the name of the Compute Engine instance the attacker accessed?



#### ❔  5: What service account is used by the Compute instance for calls to Google Cloud APIs?



#### ❔  6: In the process of exfiltrating data, identify the Google Cloud SQL database the attacker attempted to export. What is the database's name?



#### ❔  7: Tracking the data movement, which Google Cloud Storage bucket did the attacker attempt to export the database to?



#### ❔  8: In the attacker's effort to maintain access, identify the new service account created by the attacker. What is the account ID of this service account?



#### ❔  9: As part of establishing persistence, what is the ID of the secret key generated for the newly created service account?


