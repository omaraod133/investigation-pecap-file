# investigation-pecap-file:DOWNLOAD FROM FAKE SOFTWARE SITE

## Objective
[Brief Objective - Remove this afterwards]
To analyze a PCAP file obtained from Malware-Traffic-Analysis.net in order to identify malicious network activity

### Skills Learned
[Bullet Points - Remove this afterwards]

- Gained hands-on experience with packet analysis tools such as Wireshark and tcpdump.
- Improved understanding of network protocols such as HTTP, DNS, TCP, and their role in cyber attacks.
- Enhanced ability to detect and analyze malware-related network activity from real-world datasets.
- Critical Thinking: Learned to question assumptions and validate findings with evidence from packet data.

### Tools Used
[Bullet Points - Remove this afterwards]

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
first thing i like to do is to look to **statistics->conversation** to get overview of what happend

*Ref 1: statistics->conversation->ipv4*
<img width="1920" height="881" alt="Screenshot_2026-04-12_04_40_29" src="https://github.com/user-attachments/assets/ae9f1745-a949-45d0-b383-4f6c2a585680" />
here we see that ip 10.1.17.215  sending a lot of packet to foreign ip address (its likely the infected device)

Now let see the tcp conversation  to get overview of what ports is opened and see if there is any intersting ports open (like FTP,http...)
<img width="1920" height="909" alt="Screenshot_2026-04-12_04_46_23" src="https://github.com/user-attachments/assets/368448e4-e7e8-4406-b724-f0d99e83ce2f" />
here we see that there is converstion using port 80,443 from ip 10.1.17.215

then we will see the protocal that use by go to **statistics->Protocal hierarchy**
<img width="1920" height="907" alt="Screenshot_2026-04-12_05_00_25" src="https://github.com/user-attachments/assets/dac4bbbb-3af6-4279-bec9-2f6620868f27" />
here wee see what protocol has been used i the file
let answer some question
- What is the IP address of the infected Windows client?
     10.1.17.215
- What is the mac address of the infected Windows client?
     00:d0:b7:26:4a:74
<img width="1920" height="904" alt="Screenshot_2026-04-12_04_54_59" src="https://github.com/user-attachments/assets/4cef670b-31eb-4f9d-8a07-82da4b6407ce" />

- What is the host name of the infected Windows client?
  we will look to NetBois protocal see the host name of the device (we know that this protocol exsist from seeing the statistics->Protocal hierarchy)
  {NetBois protocal:is protocal that used to let device to take to eachother by using name rether then ip address} 
Example below.

*Ref 1: Network Diagram*
