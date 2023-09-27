# Research_Work
Designing a Machine Learning-Based Framework for Anomaly-based Network Intrusion Detection

==== Overview ====

The are 9 network capture datasets in total, listed below. Viol. is the security violation (Confidentiality, Integrity, and Authenticity). 


Attack Type	Attack Name	Tool		Viol.	Description: The attacker
Recon.		
   -1		OS Scan		Nmap		C	scans the network for hosts, and their operating systems, to reveal possible vulnerabilities.
   -2		Fuzzing		SFuzz		C	searches for vulnerabilities in the camera's web servers by sending random commands to their cgis.
Man in the Middle	
   -3		Video Injection	Video Jack	C,I	injects a recorded video clip into a live video stream.
   -4		ARP MitM	Ettercap	C	intercepts all LAN traffic via an ARP poisoning attack.
   -5		Active Wiretap	R.PI 3B		C	intercepts all LAN traffic via active wiretap (network bridge) covertly installed on an exposed cable.
Denial of Service	
   -6		SSDP Flood	Saddam		A	overloads the DVR by causing cameras to spam the server with UPnP advertisements.
   -7		SYN DoS		Hping3		A	disables a camera's video stream by overloading its web server.
   -8		SSL Reneg.	THC		A	disables a camera's video stream by sending many SSL renegotiation packets to the camera.
Botnet Malware	
   -9		Mirai		Telnet		C,I	infects IoT with the Mirai malware by exploiting default credentials, and then scans for new vulnerable victims network.
   
   Variable Information
   
=== The features in the csv files ===

Each row in the csv is a packet captured (chronologically). More a deep explanation, please see our paper. 
In general, each row (feature vector) are recent (temporal) statistics which describes the context of the packet's channel and its communicating parties:

Whenever a packet arrives, we extract a behavioral snapshot of the hosts and protocols which communicated the given packet. The snapshot consists of 41 traffic statistics capturing a small temporal window into: (1) the packet's sender in general, and (2) the traffic between the packet's sender and receiver.

Specifically, the statistics summarize all of the traffic...
	 ...originating from this packet's source MAC and IP address (denoted SrcMAC-IP).
	 ...originating from this packet's source IP (denoted SrcIP).
	 ...sent between this packet's source and destination IPs (denoted Channel). 	
	 ...sent between this packet's source and destination TCP/UDP Socket (denoted Socket).

A total of 23 features (capturing the above) can be extracted from a single time window Î» (see Table II). The FE extracts the same set of features from a total of five time damped windows of approximately: 100ms, 500ms, 1.5sec, 10sec, and 1min into the past (Î» = 5, 3, 1, 0.1, 0.01), thus totaling 41 features.

We note that not every packet applies to every channel type (e.g., there is no socket if the packet does not contain a TCP or UDP datagram). In these cases, these features are zeroed. Thus, the final feature vector ~x, which the FE passes to the
FM, is always a member of R^n, where n = 41.
