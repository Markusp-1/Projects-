---
share_link: https://share.note.sx/rgzo95jg#/dbVoyE2pNxYj4knPI30PUA1Z6Moc89y7WRWq66rpkw
share_updated: 2025-03-20T15:51:45-04:00
---
CyberDefenders Lab
#### Scenario

During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

____ 
Q1:  By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?
	ANS: [146.190.21.92]
	Looked into a Packet that had sent a GET request to download a file called {invoice.xml}, i followed to http stream and found that the host that sent the request was  146.190.21.92.![[Pasted image 20250203175241.png]]



Q2:  Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?
	ANS:[61616]
	The service the attacker exploited was Openwire , The port number Openwire ruins on is port 61616
	![[Pasted image 20250203180335.png]]

Q3:   Following up on the previous question, what is the name of the service found to be vulnerable?
	ANS: [Apache ActiveMQ]
	Openwire is a Protocol used by Apache ActiveMQ
	![[Pasted image 20250203180652.png]]


Q4:   The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server?
	ANS:[128.199.52.72]
	I looked through HTTP requests , and found a request to download Docker![[Pasted image 20250203181029.png]]The download was requested by 128.199.52.72


Q5:   Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?
	ANS:[Docker]
	Docker can be used to create a reverseshell on a system, Docker was downloaded on the system.
	![[Pasted image 20250205204505.png]]


Q6:   What Java class was invoked by the XML file to run the exploit?
	ANS:[java.lang.ProcessBuilder]
	![[Pasted image 20250205204523.png]]
	In the HTTP stream you can find the Java class.


Q7:   To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?
	ANS:[CVE-2023-46604]
	![[Pasted image 20250203184349.png]]
	I searched for Openwire CVE and found it.


Q8:   As part of addressing the vulnerability, the vendor implemented a validation step to prevent exploitation. Specify the Java class and method where this validation step was added.
	ANS:[BaseDataStreamMarshaller.createThrowable]
	While searching for mitigations i found a gitthub repository with the fix.
	![[Pasted image 20250205205117.png]]

___

#### What I Learned

##### Stream searching 

This is the first time i Searched HTTP and TCP streams to find information in Wireshark. Being able to follow conversations and visually see how different packets interact with each other and how they are connected gave me a new insight on reading and understanding network traffic. 




