# 🔐 Network Security Groups (NSGs) and Traffic Inspection in Azure

<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination" width="400"/>
</p>

This lab demonstrates how to inspect traffic between Azure virtual machines using Wireshark and evaluate how Network Security Groups (NSGs) affect network connectivity.

---

## 🧰 Technologies & Protocols Used

- Microsoft Azure
- Remote Desktop (RDP)
- Command-Line Tools
- Protocols: SSH, RDP, DNS, HTTP/S, ICMP, DHCP
- Wireshark (Packet Capture)

## 🖥️ Operating Systems Used

- Windows 10 (21H2)
- Ubuntu Server 20.04

---

## 🚀 High-Level Lab Flow

- Deploy Azure VMs
- Capture & Analyze ICMP Traffic
- Configure Network Security Group Firewall Rules
- Capture SSH, DHCP, DNS, and RDP Traffic

---

## 🏗️ VM Setup

Two VMs created within the same VNet/Subnet (Windows 10 and Ubuntu):

<img src="https://github.com/user-attachments/assets/33409bd8-3172-4792-8e04-09346970de02" width="600"/>

Confirmed they are both on the same network:

<img src="https://github.com/user-attachments/assets/c3fe0a2f-1129-41c5-90bc-3307c426f30a" width="300"/>
<img src="https://github.com/user-attachments/assets/57f1f3bd-098a-4adb-af47-21d045451f0a" width="300"/>

---

## 📡 ICMP Traffic Observation

Used **ping** from Windows to Ubuntu VM. Observed ICMP traffic in Wireshark:

<img src="https://github.com/user-attachments/assets/936c91a1-01fc-409e-9a37-af7ecdeb21e3" width="600"/>
<img src="https://github.com/user-attachments/assets/7f1c3637-3f04-4929-a1eb-febcbbe04f65" width="600"/>

✅ Successfully observed 4 ICMP Echo requests and 4 Echo replies.

---

## 🔥 Configuring NSG to Block ICMP

Denied ICMP traffic on Ubuntu’s NSG:

<img src="https://github.com/user-attachments/assets/66665f5f-e5c4-4fd4-948b-5ed94a89a38a" width="600"/>

Then ran perpetual ping (`ping 10.0.0.5 -t`) and observed packet drops:

<img src="https://github.com/user-attachments/assets/10f31eb1-21c9-456d-a5bb-c76ea8063014" width="600"/>

Result: **Request timed out**, confirming NSG rule blocked ICMP.

---

## 🔐 SSH Traffic Observation

<img src="https://github.com/user-attachments/assets/01ca4192-3450-4eb6-a2e4-08b673612cb3" width="250"/>

Connected to Ubuntu using:
```
ssh labuser@<private-ip>
```

Logged in, executed:
- `hostname`
- `pwd`
- `whoami`

<img src="https://github.com/user-attachments/assets/00cf2f1a-b78f-4632-8f16-6ddd9f35f01b" width="600"/>
<img src="https://github.com/user-attachments/assets/856ff1be-9ed9-414e-9d1b-9b460a4bc02a" width="600"/>

Observed SSHv2 encrypted packets in Wireshark:

<img src="https://github.com/user-attachments/assets/b3180632-ac2e-4f90-9308-7220c48e8957" width="600"/>

---

## 📦 DHCP Traffic Observation

<img src="https://github.com/user-attachments/assets/e12c9d21-5326-42f5-9114-7b33b2e7ce8b" width="200"/>

Created a batch file `dhcp.bat`:
```
ipconfig /release
ipconfig /renew
```

Ran the script from PowerShell:

<img src="https://github.com/user-attachments/assets/1d246855-afd3-452c-b6bf-545c9848ca55" width="600"/>
<img src="https://github.com/user-attachments/assets/676a15b3-30f4-4224-abf3-dc0ef32d14f6" width="600"/>

Captured full **DORA process** in Wireshark (Discover, Offer, Request, Acknowledge):

<img src="https://github.com/user-attachments/assets/3bd9bdc9-df90-4f72-b678-e0ce166b814f" width="400"/>

Filter used: `udp.port == 67 || udp.port == 68`

---

## 🌐 DNS Traffic Observation

<img src="https://github.com/user-attachments/assets/d7157dc6-8b70-4904-b399-0e598c7eb9ef" width="600"/>

### DNS Example 1 - Failure
- Packet 87–88: Query for proxy-related name → `No such name` response

### DNS Example 2 - Success
- Packet 1980–1981: Query for Microsoft telemetry → Resolved

<img src="https://github.com/user-attachments/assets/fcc41eaf-0090-46d7-a5c2-3719638885c4" width="600"/>

### DNS Example 3 - Disney.com
Ran:
```
nslookup www.disney.com
```

Observed:
- CNAME: video.disney.com.edgesuite.net
- CNAME: a1996.dscf1.akamai.net

<img src="https://github.com/user-attachments/assets/ea54f014-9ee1-4e19-b575-d9608888fc12" width="600"/>

---

## 💻 RDP Traffic Observation

<img src="https://github.com/user-attachments/assets/6d34f37d-3e1e-43ae-9e90-283baf503477" width="600"/>

RDP allows remote access to Windows machines. Connection from external client to Azure-hosted Windows VM was successful.

---

## ✅ Summary

This lab demonstrated real-time packet captures and analysis using Wireshark. We:

- Captured and analyzed traffic for ICMP, SSH, DHCP, DNS, and RDP
- Configured NSGs to enforce network security rules
- Used command-line tools and scripting to simulate and automate behavior

















