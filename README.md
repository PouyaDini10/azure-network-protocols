

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

<img width="175" alt="image" src="https://github.com/user-attachments/assets/01ca4192-3450-4eb6-a2e4-08b673612cb3" />


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

**What is a DHCP network protocol?**

DHCP is a network protocol used to automatically assign IP addresses and other network settings(such as subnet mask,gateway,DNS) to devices on a network.

**DORA Process:**

- Discover(Client asks for an IP)
- Offer(Server offers IP)
- Request(Client requests the offered IP)
- Ask(Server acknowledges and assigns it)

The DORA process will be useful when analyzing the packet captures within wireshark.

For this segment of the networking lab, I will establish a new IP address from my Windows 10 VM using the command line for DHCP analysis. On the Powershell interface, I will execute two notable commands: ipconfig /release and ipconfig /renew. During this process, we will observe the generated DHCP traffic. 
<br><br><br>
**Using PowerShell to Locate and Run a Custom DHCP Batch Script**


Although there are a few other techniques for resetting DHCP activity, I will use a batch file method for this lab. Essentially, what I am going to do is in notepad I will write this: 

ipconfig /release
ipconfig /renew

I saved it on my C drive under Program Data and named the document dhcp.bat to quickly identify the batch file.

In Powershell, access your document by inputting cd c:/programdata followed by ls(command to list files). You should see the batch document at the bottom of the list of contents displayed.

![image](https://github.com/user-attachments/assets/1d246855-afd3-452c-b6bf-545c9848ca55)
<br><br><br>
**Automating DHCP Lease Renewal with Batch Scripting and Network Traffic Analysis**

![image](https://github.com/user-attachments/assets/676a15b3-30f4-4224-abf3-dc0ef32d14f6)

<br><br><br><br>

**Step-by-Step Overview of the DHCP Lease Process (DORA + Release)**

<img width="356" alt="image" src="https://github.com/user-attachments/assets/3bd9bdc9-df90-4f72-b678-e0ce166b814f" />
<br><br><br><br>
Some key points to highlight when assessing your current operations: to filter for DHCP traffic, it is appropriate to utilize this query: udp.port == 67 || udp.port == 68
<br><br>
The command .\dhcp.bat will begin the entire DORA process from start to finish. Allow some time, and you'll soon see DHCP network traffic in Wireshark. Upon further investigation, it's common for the server to issue back the same IP address as the one we had before. 

<br><br><br><br>

**Observe DNS(Domain Name System) Traffic**


<img width="488" alt="image" src="https://github.com/user-attachments/assets/d7157dc6-8b70-4904-b399-0e598c7eb9ef" />


DNS translates domain names(google.com) into IP addresses(8.8.8.8) so your computer can find and communicate with the correct server. It's because of DNS servers humans don't have to memorize IP addresses. 

For example, suppose a user is unable to connect to www.XYZ.com(an example website) in most cases. In that case, it's a DNS issue, not an internet issue, and we can perform a fundamental network analysis, such as utilizing nslookup or ping to analyze the network traffic. 

<br><br>

DNS observation:
- Filter for DNS traffic on Wireshark
- Observe a specific packet capture
- Perform a basic nslookup to any website


<br><br>
**DNS Traffic Analysis Using Wireshark: Real-Time Query and Response Inspection**

![image](https://github.com/user-attachments/assets/fcc41eaf-0090-46d7-a5c2-3719638885c4)

To clarify, this is a very standard DNS request and response communication between 10.0.0.4(Windows 10 VM) and 168.63.129.16(Azure DNS Server). 

Assessing the first network packet capture, 87 & 88, you can see that this DNS lookup failed. Packet 87 displays 10.0.0.4, requesting," What is the IP address for: wpad.r44m1rjgyo2u3pbzgd1iruknq?" The DNS server responds, " No such name exists." This is a common issue within the parameters of proxies. 

Moving forward to the following packet capture, 1980 & 1981, our virtual machine sent in a request to identify the IP address for v10.events.data.microsoft.com. The server responded with the appropriate response. 

This demonstrates both a typical failed DNS lookup related to proxy auto-discovery and a successful query for a legitimate Microsoft service. Understanding these responses is essential for identifying normal vs. abnormal network behavior.
<br><br>
**DNS Query Analysis: www.disney.com**

To further our understanding of DNS fundamentals, I want to analyze network traffic for disney.com. A simple PowerShell command, "nslookup www.disney.com," will do the trick. From there, we will see what appears in Wireshark. 
<br><br><br><br>
![image](https://github.com/user-attachments/assets/ea54f014-9ee1-4e19-b575-d9608888fc12)
<br><br><br><br>

**Phase 1: Internal DNS Attempts (Failed)**
At first, my computer (10.0.0.4) tried to find the IP address for www.disney.com by checking internal network versions of the domain, such as:

www.disney.com.r44m1rjgyo2u3pbzgd1iruknqf.xx.internal.cloudapp.net
www.disney.com.xx.internal.cloudapp.net
www.disney.com.internal.cloudapp.net

On each attempt, the DNS server responded with, "No such name."  These kinds of internal checks are common in cloud environments like Microsoft Azure. They're just your system checking if the domain exists locally before going to the Internet.

**Phase 2: Real DNS Response (Success)**
In packet **105024**, the real query for www.disney.com was finally sent out.

The DNS server responded with:

A CNAME (alias) pointing to:
video.disney.com.edgesuite.net

Then another CNAME pointing to:
a1996.dscf1.akamai.net

**Observe RDP(Remote Desktop Protocol) Traffic**


<img width="512" alt="image" src="https://github.com/user-attachments/assets/6d34f37d-3e1e-43ae-9e90-283baf503477" />


RDP stands for Remote Desktop Protocol and it is a Microsoft protocol that allows you to remotely connect and control another Windows computer or server(as if you were sitting in front). 

















