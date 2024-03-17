<h1>Wireshark-PacketAnalysis</h1>

<h2>Description</h2>
RedLine Stealer is information-stealing malware that harvests login credentials and other sensitive data from a victim's Windows host. This Wireshark project uses a packet capture (pcap) that “crosses a line” separating normal traffic from malicious activity. The malicious activity in this pcap is a RedLine Stealer infection from July 2023. The pcap provides experience analyzing RedLine traffic, and we can determine what specific data was stolen from an infected Windows computer.

<h2> Aims </h2>

1. What is the date and time in UTC the infection started?</b> 

2. What is the IP address of the infected Windows client?</b> 

3. What is the MAC address of the infected Windows client?</b> 

4. What is the hostname of the infected Windows client?</b> 

5. What is the user account name from the infected Windows host?</b> 

6. What insights can be gained from analyzing the HTTP GET requests made by the RedLine Stealer malware, and how do these requests indicate the malware's behavior and potential actions on the infected system?</b> 

<h2>Languages and Utilities Used</h2>
- <b>Wireshark</b> 

<h2>Program walk-through:</h2>

<p align="center">

Applying the basic web filter reveals a single internal IP address at 10.7.10[.]47 as the only source IP address, as shown below in Figure 1. Using the frame details, we can correlate this source IP with a MAC address at 80:86:5b:ab:1e:c4. From the information obtained below we can see the infection started on July 10, 2023, at 22:39 UTC.

<img src="https://imgur.com/QzmiDHp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

NBNS is a protocol used to resolve NetBIOS names to IP addresses, primarily in Windows networks. From figure 2 below we can see the hostname is DESKTOP-9PEA63H. It is not COOLWEATHERCOAT as it is the domain name rather than the specific host name. 

<img src="https://imgur.com/gbjgIUx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

We can verify the victim’s hostname and Windows user account name through Kerberos authentication traffic. Filter on kerberos.CNameString to find the Windows user account name rwalters, as shown below in Figure 3.

<img src="https://imgur.com/2AQN9ld.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

The HTTP GET requests captured in the Wireshark traffic reveal important information about the RedLine Stealer infection. Let's break down what these GET requests show and mean:

http://623start[.]site/?status=start&av=Windows%20Defender
If we follow the TCP stream, this GET request indicates the malware attempting to start its malicious activities. The status=start parameter likely triggers the malware to initiate its operations. The av=Windows%20Defender parameter suggests that the malware is checking for the presence of Windows Defender antivirus on the victim's system.

<img src="https://imgur.com/blMdpes.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

http://623start[.]site/?status=install
This GET request signifies another stage in the malware's execution. The status=install parameter could imply that the malware is attempting to install additional components or perform certain actions on the infected system.

<img src="https://imgur.com/a0qfID1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

http://guiatelefonos[.]com/data/czx.jpg
This GET request points to a URL where the malware attempts to fetch additional resources or execute further commands. In this case, it's requesting a JPEG file.
The presence of a file extension in the URL (czx.jpg) suggests that the server may respond with a file download, potentially containing malicious code or instructions.

<img src="https://imgur.com/pyYX6oG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

These GET requests provide insights into the behavior of the RedLine Stealer malware, including its attempt to start, install, and fetch additional resources from remote servers. Analyzing such network traffic helps security analysts understand the malware's tactics and devise appropriate countermeasures to mitigate the threat.



<img src="https://imgur.com/pyYX6oG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

