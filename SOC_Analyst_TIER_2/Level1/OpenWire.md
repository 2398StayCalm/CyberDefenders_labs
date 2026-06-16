# 💻 OpenWire
📌Link to the lab -> [https://cyberdefenders.org/blueteam-ctf-challenges/openwire/]

### 📓 Scenario:
During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

### ❓Questions❓:

#### ❔  1: By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?

Going to Statistics -> Conversations I can see that one conversation stands out with a 5MB packets. Now which one would be a compromised server is visible with the first 3 packets from a PCAP file. The connection is initiated by one of these IP with SYN.

<img width="1267" height="737" alt="image" src="https://github.com/user-attachments/assets/809fe3cf-54e1-4567-a339-607f75992edf" />

It worked! 🔥

#### ❔  2: Initial entry points are critical to trace the attack vector back. What is the port number of the service the adversary exploited?

The initiation between a compromised server and the victim show the port that was used for a successful communication in the previous answer/screenshot. It is in the first packet even.

It worked! 🔥

#### ❔  3: Following up on the previous question, what is the name of the service found to be vulnerable?

I've decided to follow TCP stream of the packet related to the compromised server and initial entry point. There is a provider name which should be checked using OSINT/online research. Sounds promising especially because it is an open-source message broker that enables applications, services, and microservices to communicate asynchronously through messaging queues and topics. It also:
- processes XML
- supports dynamic class loading
- is often exposed to the Internet

<img width="847" height="202" alt="image" src="https://github.com/user-attachments/assets/6765dbe6-300e-4f2a-ab0b-630fda1c76f1" />

It worked! 🔥

#### ❔  4: The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server?

I went back to Statistics -> Conversations and I can see that there are 2 more different conversations which include the victim. One of them is actually showing attempts to repeatedly establish HTTPS connection which end up failing which is shown in the screenshot below. So it suggests that the third conversation include the second Compromised C2 server.

<img width="1295" height="357" alt="image" src="https://github.com/user-attachments/assets/d934bd7d-dd75-4710-8f07-32768a6c758f" />

It worked! 🔥

#### ❔  5: Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?

In the conversation between first found compromised server I can see HTTP protocols which point to victim making request for a malicious XML file. The XML file contains a Spring Bean definition. Instead of defining a normal application bean, the XML instructs to instantiate java.lang.ProcessBuilder and immediately executes it. Then the whole fun begins, because the ProcessBuilder executes a command which downloads a payload, saves it in a /tmp directory then gives executable privileges and executes it.

<img width="1282" height="652" alt="image" src="https://github.com/user-attachments/assets/f706e4fa-a09e-41fa-b6e7-aaf94c86ddcd" />

It worked! 🔥

#### ❔  6: What Java class was invoked by the XML file to run the exploit?

This is included in the last answer. It is responsible for immediate execution.

It worked! 🔥

#### ❔  7: To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?

Doing a little online research and the first result gave the answer.

<img width="1135" height="352" alt="image" src="https://github.com/user-attachments/assets/e45cf4db-c9ee-4627-8645-28f8c23b41c1" />

It worked! 🔥

#### ❔  8: The vendor addressed the vulnerability by adding a validation step to ensure that only valid Throwable classes can be instantiated, preventing exploitation. In which Java class and method was this validation step added?

The link from the question takes us to the Github repository. The class that interests me is the first public class that is created in the 1st version. Then after describing the class, the method Throwable is configured taking 2 arguments in its function.

<img width="1062" height="197" alt="image" src="https://github.com/user-attachments/assets/6f1b13a9-5d43-49a8-967e-cee82d2c2abb" />
<img width="1295" height="342" alt="image" src="https://github.com/user-attachments/assets/547da2cb-dad6-4540-94a4-6303657732a7" />

It worked! 🔥

All done! 🥇
