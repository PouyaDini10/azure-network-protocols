<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />



- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create our Virtual Machines
- Observe ICMP traffic
- Configuring a Firewall Network Security Group
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

Created and configured two virtual machines (Windows 10 and Linux(Ubuntu)) within the same virtual network and subnet to simulate real-world network environments.This step established the base environment for future hands-on networking labs.

<img width="477" alt="image" src="https://github.com/user-attachments/assets/33409bd8-3172-4792-8e04-09346970de02" />

I would then ensure that both virtual machines function on the same virtual network. This step is crucial for successful network operations.

<img width="222" alt="image" src="https://github.com/user-attachments/assets/c3fe0a2f-1129-41c5-90bc-3307c426f30a" />
<img width="203" alt="image" src="https://github.com/user-attachments/assets/57f1f3bd-098a-4adb-af47-21d045451f0a" />

Then, I would establish a remote desktop connection with the Windows 10 virtual machine using its public IP address. Because I'm outside of Azure’s network, a public IP address would be appropriate on establishing a remote connection using my Windows machine. 



Within my Windows virtual machine, I'm going to open wireshark which is a network protocol analyzer used to capture network traffic and we're going to initiate a simple packet capture. 

Key network protocols to be mindful of:

TCP - a connection oriented, reliable, and ordered network protocol that ensures all data is error checked prior to transmission.

UDP - a connectionless network protocol, used for fast and lightweight transmission of data with minimal overhead.

DNS - establishes IP addresses for domain names(www.google.com).

DHCP - Assigns IP addresses to devices on a network automatically. 

ICMP - Used for diagnostic purposes, e.g., pinging another device on the network.

SSH - Securely connects to remote systems (commonly used for command-line access).

![image](https://github.com/user-attachments/assets/c722b5ed-1759-43a0-8d90-6f4c7df8974b)


</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
