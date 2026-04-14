# investigation-pecap-file:DOWNLOAD FROM FAKE SOFTWARE SITE

## Objective
To analyze a PCAP file obtained from Malware-Traffic-Analysis.net in order to identify malicious network activity

### Skills Learned
- Gained hands-on experience with packet analysis tools such as Wireshark and tcpdump.
- Improved understanding of network protocols such as HTTP, DNS, TCP, and their role in cyber attacks.
- Enhanced ability to detect and analyze malware-related network activity from real-world datasets.
- Critical Thinking: Learned to question assumptions and validate findings with evidence from packet data.

### Tools Used
- virustotal (website)
- alienvault
- Network analysis tools (Wireshark) for analsing the pcap file.
## explanaion of the pcap:

<img width="1897" height="791" alt="{E4968E87-601E-489D-A130-FB33D2C90C57}" src="https://github.com/user-attachments/assets/12d3d654-3a26-464d-b580-730f5f1cce51" />

 and we shoud answer these questions:
 
- What is the IP address of the infected Windows client?
- What is the mac address of the infected Windows client?
- What is the host name of the infected Windows client?
- What is the user account name from the infected Windows client?
- What is the likely domain name for the fake Google Authenticator page?
- What are the IP addresses used for C2 servers for this infection?
## Steps
The first thing I like to do is check Statistics → Conversations to get an overview of the network activity.

*Ref 1: statistics->conversation->ipv4*
<img width="1920" height="881" alt="Screenshot_2026-04-12_04_40_29" src="https://github.com/user-attachments/assets/ae9f1745-a949-45d0-b383-4f6c2a585680" />
Here, we can see that the IP address 10.1.17.215 is sending a large number of packets to external IP addresses. This behavior suggests that it is likely the infected device.

Next, let’s examine the TCP conversations to identify which ports are in use and check for any interesting activity (such as FTP or HTTP).

<img width="1920" height="909" alt="Screenshot_2026-04-12_04_46_23" src="https://github.com/user-attachments/assets/368448e4-e7e8-4406-b724-f0d99e83ce2f" />
We can see communication over ports 80 (HTTP) and 443 (HTTPS) from the IP address 10.1.17.215.

Next, we identify the protocols in use by navigating to Statistics → Protocol Hierarchy.

<img width="1920" height="907" alt="Screenshot_2026-04-12_05_00_25" src="https://github.com/user-attachments/assets/dac4bbbb-3af6-4279-bec9-2f6620868f27" />

Now, let’s answer the questions:
#### What is the IP address of the infected Windows client?
10.1.17.215
#### What is the MAC address of the infected Windows client?
00:d0:b7:26:4a:74
<img width="1920" height="904" alt="Screenshot_2026-04-12_04_54_59" src="https://github.com/user-attachments/assets/4cef670b-31eb-4f9d-8a07-82da4b6407ce" />

#### What is the host name of the infected Windows client?
To find this, we look at the NetBIOS protocol, which we identified earlier in Statistics → Protocol Hierarchy.
NetBIOS is a protocol that allows devices to communicate using names rather than IP addresses.

So, we will search for nbns (this is the protocol used by NetBIOS).

<img width="1920" height="917" alt="Screenshot_2026-04-14_01_56_51nbns" src="https://github.com/user-attachments/assets/b0263dbb-ec88-49d1-b715-743acf84a4e5" />

The answer to the question is: **DESKTOP-L8C5GSJ**

#### - What is the user account name from the infected Windows client?
Kerberos is a protocol used to allow users to access services securely without sending their password every time. It is commonly used in Windows Active Directory
Since authentication is required, the username is included in the traffic.

<img width="1920" height="914" alt="Screenshot_2026-04-14_02_12_51kerbers" src="https://github.com/user-attachments/assets/f1343afe-5c09-4b87-8f2f-5bb7d2ace92e" />
The user account name is: shutchenson

#### - What is the likely domain name for the fake Google Authenticator page?
We can find this by analyzing DNS or TLS communication.
I will search in the TLS protocol because there are fewer packets, making it easier to analyze.

<img width="1920" height="907" alt="Screenshot_2026-04-14_02_38_30" src="https://github.com/user-attachments/assets/309eb78b-cabd-4d8e-a772-6a9936d59c0a" />

Here, we will add server_name as a column

<img width="1920" height="947" alt="Screenshot_2026-04-14_02_47_45" src="https://github.com/user-attachments/assets/f1dd44a6-49a0-4772-82e6-f589329b5f1f" />
Now we can search for suspicious domain names.

<img width="1920" height="911" alt="Screenshot_2026-04-14_04_04_46" src="https://github.com/user-attachments/assets/f9c33d78-1b30-4281-ad32-39ac65b20365" />
here we see these suspicious domain
Next, I checked these domains using VirusTotal to see if they are suspicious.

<img width="1918" height="850" alt="{4BACFD79-4ACA-41B7-9712-E4E5D9E3178F}" src="https://github.com/user-attachments/assets/70fec421-a366-4ab7-ac75-bc745fd4c66e" />

The likely domain name for the fake Google Authenticator page is:
**google-authenticator.burleson-appliance.net**

#### - What are the IP addresses used for C2 servers for this infection?
We will go back to the TLS protocol to check for any unusual domain names.
I found something interesting: a server using an IP address as a domain name, which may indicate the attacker is trying to bypass DNS filtering.
Additionally, there are repeated requests sent every 5 seconds, which is a strong indicator of beaconing behavior commonly associated with C2 (Command and Control) communication.

<img width="1920" height="947" alt="Screenshot_2026-04-14_04_13_41" src="https://github.com/user-attachments/assets/27bd91c5-8362-46b8-8bfb-40df2140f315" />

When searching this IP address (45.125.66.32,5.252.153.241) on OTX AlienVault, we can see under Related Pulses that there are multiple reports associated with it.

<img width="1915" height="855" alt="{A68CB8E6-CBE0-455A-8B10-3AB5A1E39C57}" src="https://github.com/user-attachments/assets/3d382430-a30b-4556-b8ca-8f217ef65600" />

So, the IP address of the C2 server is: 45.125.66.32,5.252.153.241

let dig deepr
