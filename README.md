

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

**Setting up the virtual machines:**

Created and configured two virtual machines (Windows 10 and Linux(Ubuntu)) within the same virtual network and subnet to simulate real-world network environments.This step established the base environment for future hands-on networking labs.

<img width="477" alt="image" src="https://github.com/user-attachments/assets/33409bd8-3172-4792-8e04-09346970de02" />

I would then ensure that both virtual machines function on the same virtual network. This step is crucial for successful network operations.

<img width="222" alt="image" src="https://github.com/user-attachments/assets/c3fe0a2f-1129-41c5-90bc-3307c426f30a" />
<img width="203" alt="image" src="https://github.com/user-attachments/assets/57f1f3bd-098a-4adb-af47-21d045451f0a" />

Then, I would establish a remote desktop connection with the Windows 10 virtual machine using its public IP address. Because I'm outside of Azure’s network, a public IP address would be appropriate on establishing a remote connection using my Windows machine. 

**Wireshark Packet Capture:**

Within my Windows virtual machine, I'm going to open wireshark which is a network protocol analyzer used to capture network traffic and we're going to initiate a simple packet capture. 

Key network protocols to be mindful of:

TCP - a connection oriented, reliable, and ordered network protocol that ensures all data is error checked prior to transmission.

UDP - a connectionless network protocol, used for fast and lightweight transmission of data with minimal overhead.

DNS - establishes IP addresses for domain names(www.google.com).

DHCP - Assigns IP addresses to devices on a network automatically. 

ICMP - Used for diagnostic purposes, e.g., pinging another device on the network.

SSH - Securely connects to remote systems (commonly used for command-line access).

**Observe ICMP Traffic:**

Now that we have a good understanding of how wireshark operates, the next plan of action would be to analyze ICMP traffic. The network protocol ICMP is what ping uses and it’s a unique protocol that can be used for diagnostic purposes. What do I mean by this, well simply put ICMP can be thought of as like a messenger. For instance, is google reachable using our device. 

For good practicality, lets try and communicate with our Linux virtual machine. To do this we will first need the Linux VM’s private IP address and we will use the ping command in powershell to commence the operation. Afterwards, in Wireshark filter for ICMP traffic and observe.

![image](https://github.com/user-attachments/assets/936c91a1-01fc-409e-9a37-af7ecdeb21e3)


As you may have noticed, we used the ping command from our source(10.0.0.4) to reach the destination(10.0.0.5) and the execution was successful. Further analyzing the details, we sent 4 Echo ICMP Requests and we received 4 Echo replies from the Linux machine. With a 0% loss and at a fast execution rate. We can conclude that our Windows VM(10.0.0.4) can successfully reach the Linux VM(10.0.0.5) with a fast and stable connection.

![image](https://github.com/user-attachments/assets/7f1c3637-3f04-4929-a1eb-febcbbe04f65)


**Configuring a Firewall(NSG's(Network Security Groups)):**

To make this lab a little interesting, I want to now observe network connectivity if a security procedure was established in the Linux VM’s network settings and then observe the ICMP traffic. This process is simple, on Azure you will access the Linux VM’s Network Security Group’s and deny any incoming ICMP traffic. Should look something like this:

<img width="434" alt="image" src="https://github.com/user-attachments/assets/66665f5f-e5c4-4fd4-948b-5ed94a89a38a" />

Now back in our virtual environment, initiate a perpetual ping command(ping 10.0.0.5 -t) and now observe the ICMP traffic:

Note - a perpetual ping command is where you're requesting information non-stop until you manually disable it.

![image](https://github.com/user-attachments/assets/10f31eb1-21c9-456d-a5bb-c76ea8063014)

As you can see, the Windows VM(10.0.0.4) sent in some requests, the requests were sent successfully, but the Linux VM(10.0.0.5) ignored the requests. In our powershell interface, we can see this with the "Request timed out" message. We can safely conclude we have not established a feasible connection to our Linux virtual machine.  


**Observe SSH(Secure Shell) Traffic:**

<img width="441" alt="image" src="https://github.com/user-attachments/assets/1fae5267-c304-416b-930b-d7de624dd563" />

In this portion of the lab, we will examine SSH traffic. A quick definition: SSH(Secure Shell) is used to connect securely from one computer to another over an unsecured network.

In essence, SSH provides a secured encrypted tunnel so you can:
- Remotely log in to another computer
- Secure File Transfers
- Run Commands
- Automation

To get started, we will need to remotely connect to our Linux VM through powershell and the command prompt would look something like this: **ssh labuser@ private IP address**
Once you input all the specifications correctly, you will be commanded to enter the password for the username of the Linux virtual machine. You will notice immediately that when entering the password, you won't see the characters being written, but it is being typed. Once you log in, your screen should read:

![image](https://github.com/user-attachments/assets/00cf2f1a-b78f-4632-8f16-6ddd9f35f01b)


To clarify, we are using the SSH(Secure Shell) protocol to gain remote access to our Linux machine on the same private network. A private IP address would be feasible because our Azure virtual machines exist on the same virtual network.  

From here, execute some commands like:
- hostname
- pwd
- whoami

![image](https://github.com/user-attachments/assets/856ff1be-9ed9-414e-9d1b-9b460a4bc02a)

Now let's observe SSH traffic; opening Wireshark will immediately commence packet captures. Initiate a command in PowerShell, like "hostname," and observe the produced SSH traffic. Under SSH protocol, there will be an encrypted packet, but the payload is unaccessible. Here, upon careful observation, all the details of the packet capture are encrypted. 

**This screenshot shows secure SSHv2 traffic between two Azure VMs. The data is encrypted end-to-end, demonstrating secure remote access and traffic analysis using Wireshark.**
![image](https://github.com/user-attachments/assets/b3180632-ac2e-4f90-9308-7220c48e8957)

**Observe DHCP(Dynamic Host Configuration Protocol) Traffic:**

<img width="200" alt="image" src="https://github.com/user-attachments/assets/e12c9d21-5326-42f5-9114-7b33b2e7ce8b" />

DHCP is a network protocol used to automatically assign IP addresses and other network settings(such as subnet mask,gateway,DNS) to devices on a network.


**Observe DNS(Domain Name System) Traffic**


<img width="488" alt="image" src="https://github.com/user-attachments/assets/d7157dc6-8b70-4904-b399-0e598c7eb9ef" />


DNS translates domain names(google.com) into IP addresses(8.8.8.8) so your computer can find and communicate with the correct server. 



**Observe RDP(Remote Desktop Protocol) Traffic**


<img width="512" alt="image" src="https://github.com/user-attachments/assets/6d34f37d-3e1e-43ae-9e90-283baf503477" />


RDP stands for Remote Desktop Protocol and it is a Microsoft protocol that allows you to remotely connect and control another Windows computer or server(as if you were sitting in front). 

















