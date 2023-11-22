
<h1>Hping3-Attack-and-Analysis-of-Pcap</h1>

<h2>Description</h2>
Red Side Attack - SYN Flood:
In the second deliverable of this cybersecurity project, I executed a simulated TCP SYN Flood attack using Kali Linux and hping3 on my own computer. The objective was to demonstrate the ease with which attackers can inundate a system with a barrage of malicious TCP packets, causing a surge in CPU usage and initiating a successful attack.
Blue Team Analysis - Incident Response:

<br />


<h2>Languages and Utilities Used</h2>

- <b>Linux</b> 
- <b>Wireshark</b>
- <b>Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2>Project walk-through:</h2>

<p align="center">
Launching the Attack (Step 1): <br/>
<img src="https://i.imgur.com/thllXfg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Launching the Attack (Step 2)  <br/>
<img src="https://i.imgur.com/daUmmox.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Executing command for the attack: <br/>
<img src="https://i.imgur.com/skk2SmI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Increase in CPU utilization:  <br/>
<img src="https://i.imgur.com/PBE2brG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<h2>Steps required to complete the attack</h2>

<ul>
  <li>Using the command prompt and typing “ipconfig” I made a note of my IP Address.</li>
  <li>Opened Kali Linux using Virtual Box. First, we must install hping3 using the “sudo-apt get install hping3”. Refer to the screenshot below.</li>
  <li>3.	Next, I used the following command. hping3 -c 15000 -d 120 -S -w 64 -p 80 --flood --rand-source 10.178.56.229. There might be an error of “can't open raw socket”. For this, we must first use “sudo -s” which is used to gain elevated privilege. Refer to the screenshot below.</li>
  <li>4.	Let's break down the above command. -c 15000 means we are sending 15000 packets. -d 120 means each packet is 120 bytes. -S says SYN flag is enabled with a TCP window size of 64(-w 64), we are directing the attack on port 80 with -p 80. –flood indicates sending packets as fast as possible. --rand source helps with spoofed IP addresses to disguise the real source. </li>
  </ul>

<h1>Blue Team Analysis</h1>
<h2>Description</h2>
In response to the SYN Flood attack, the Blue Team conducted an incident analysis on a suspicious PCAP file received on November 10, 2023. The analysis was led by Hammaz Ahmed, the Incident Response Analyst.
<br />
<h2>Project walk-through:</h2>

<p align="center">
Wireshark Dashboard: <br/>
<img src="https://i.imgur.com/Ahj1Gnm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wireshark Dashboard:  <br/>
<img src="https://i.imgur.com/XtCV3eJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wireshark Dashboard: <br/>
<img src="https://i.imgur.com/nKzbB37.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<h2>Step-by-Step Analysis</h2>
<ul>
  <li>	First, I took a scroll glance at the whole pcap file. It had some red flags for the TCP stream. Hence, I filtered by typing “tcp” in the filter column. Not much can be drawn from just this.</li>
  <li>	I used the filter “tcp.flags.syn==1 && tcp.flags.ack==0”. This filter helps in TCP packets that are part of the 3-way handshake. Missing a proper 3-way handshake can give us a hint of synflood attacks. In fact, we do see loads of synchronization packets but no sign of a complete handshake.</li>
  <li>	I can also see a sudden rise in packets and continuously receiving similar ones. This is also an indicator of a synflood attack.</li>
  <li>		If we look at the time interval in which the packets are coming, it is suspicious since the time gap is very low. That is an extremely large number of requests occurring in a brief interval of time. </li>
   <li>	I can also see an increase in “TCP Spurious Transmission”. The receiver is receiving a retransmitted segment even before the ACK packet is sent. This can be an indicator of a synflood attack. The below screenshot also highlights “TCP dup ACK” which shows the arrival of multiple ACK packets. This is usually due to network congestion, or packet loss (another indicator of a SYN Flood Attack)</li>
  </ul>

  <h2>Conclusion</h2>
  
<li>Sudden Increase in SYN Packets. </li>
  <li>Incomplete 3-way Handshakes.</li>
  <li>TCP Spurious Transmissions and TCP dup ACK.</li>
  <li>	Large number of similar traffic within a small time frame.</li>
  After critically investigating the pcap file it is clear that there has been a SYN Flood Attack (Discussed above) since there are several indicators. Noticing the severity and unambiguousness of this event, it will be reassigned to the Incident Response Team Manager. The incident was analyzed but needs further investigation and clarification to point out the severity and if any denial of service happened. A follow-up report will be generated for this event.
  
  </ul>
  <h2>Recommendations</h2>
  This type of attack overwhelms the network by sending tons of connection requests, potentially leading to disruption of service. While our team is actively monitoring the situation, I recommend some countermeasures.
•	Firewalls: Configure your firewall to detect and block malicious SYN flood traffic. This may involve setting up rules to block traffic from specific source IP addresses or implementing heuristics to identify abnormal traffic patterns.
•	TCP Timeout Adjustment: Adjust the TCP timeout values on your server. By tweaking these values, you can potentially reduce the impact of SYN flood attacks by releasing half-open connections more quickly.
•	Syn Cookies: Enable SYN cookies on our server. SYN cookies are a technique that allows the server to validate connection requests without maintaining a full connection state until the three-way handshake is complete. This can help mitigate the impact of a SYN flood attack.
•	Improve Network Monitoring: Strengthen our network monitoring capabilities to promptly detect and respond to any unusual patterns or irregularities in our traffic. 
•	Review Security Policies: Regularly review and update our security policies to ensure they align with the latest best practices and are effective against evolving threats.
•	Utilize load balancers to distribute incoming traffic across multiple servers. This can help distribute the impact of a SYN flood attack, making it more difficult for the attacker to overwhelm a single server.


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
