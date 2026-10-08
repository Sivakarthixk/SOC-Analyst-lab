**# Network Traffic Analysis**



**## 1. Investigation Overview**



Investigation ID: SOC-001



Tool Used: Wireshark



Objective:

Analyze network traffic captured from a Windows workstation and

identify normal network communication and potentially suspicious activity.



**## 2. Environment**



Operating System: Windows 11

Network Interface: Wi-Fi

Tool: Wireshark



**## 3. Investigation Process**



1\. Started Wireshark.

2\. Selected the active network interface.

3\. Captured network traffic for approximately 1–2 minutes.

4\. Generated normal web and DNS traffic.

5\. Applied Wireshark display filters.

6\. Analyzed DNS and TCP traffic.

7\. Investigated source and destination IP addresses.



**## 4. DNS Analysis**



Display Filter:



dns



Observation:



DNS packets were observed during the capture. DNS traffic was

associated with domain-name resolution.



Port Observed:



53



Assessment:



Port 53 is commonly used for DNS communication. The observed

traffic appeared consistent with normal DNS activity.



**## 5. TCP Analysis**



Display Filter:



tcp



Observation:



TCP traffic was observed between the workstation and remote hosts.



**## 6. Connection Analysis**



Source IP:



\[10.236.182.192]



Destination IP:



\[10.236.186.131]



Protocol:



\[DNS - DOMAIN NAME SERVER]



Destination Port:



\[53]



**## 7. Findings**



The captured traffic contained normal DNS and TCP communication.

No malicious activity was confirmed from the traffic analyzed.



**## 8. Recommendations**



\- Continue monitoring unusual network connections.

\- Investigate abnormal DNS query patterns.

\- Monitor unexpected external destinations.

\- Use additional security logs and threat intelligence for deeper investigation.



**## 9. Conclusion**



The investigation demonstrated the process of capturing and analyzing

network traffic using Wireshark. DNS and TCP communications were

examined to understand normal network behavior and identify indicators

that could require further investigation.

