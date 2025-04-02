---
share_link: https://share.note.sx/lh19v2a9#H/nnRz159j+AcmL6XfaOqZhmR8wG0nWKWbaZpFpWMhE
share_updated: 2025-04-01T16:18:07-04:00
---
#### Tools 

Wireshark

#### Scenario

An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.


Q1: To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?
[ANS] : 10.0.0.130

Checking through the IPV4 conversations I found Unusual amounts of traffic Between 10.0..0.130 and 10.0.0.133
![[Pasted image 20250401135026.png]]
![[Pasted image 20250401135147.png]]
I found packets relating to file creation and closer and auth packets, there were also packets that were file creations For PSEXESVC which all came from the attacking machine 10.0.0.130 


---

Q2: To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?
[ANS] : Sales-Pc

I followed the tcp stream of the auth packet and found the hostname.

![[Pasted image 20250401135708.png]]

___


Q3: Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

[ANS] : ssales 
![[Pasted image 20250401135708.png]]

---

Q4:After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?
[ANS] : PSEXESVC
![[Pasted image 20250401140630.png]]

---

Q5: We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?
[ANS] : ADMIN$

![[Pasted image 20250401141431.png]]


Q6: We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?
[ANS] : IPC$
![[Pasted image 20250401141522.png]]

---

Q7:Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?
[ANS] : Marketing-Pc

In the ipv4 conversations the second machine that had contact with the attacker machine was 10.0.0.131  

It had similar packets the the first compromised machines.
![[Pasted image 20250401141731.png]]

So I followed the tcp stream and It lead me to the hostname of the second compromised machine.

![[Pasted image 20250401141925.png]]
