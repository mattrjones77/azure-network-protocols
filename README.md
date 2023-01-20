<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

1. Create Resources
2. Observe ICMP Traffic
3. Observe SSH Traffic
4. Observe DHCP Traffic
5. Observe DNS Traffic
6. Observe RDP Traffic
7. Cleanup

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/A8glNGZ.png" height="80%" width="80%" alt=""/>
</p>
<p>
1.) In the Azure portal, create a Resource Group along with two Virtual Machines (VM). One VM will be a Windows 10 with 1-2 vCPUs, and the other will be Ubuntu (Linux) with 1-2 vCPUs as well. Make sure both VMs are in the same VNet. To view the Topology in Network Watcher, you may need to move the NetworkWatcherRG to the newly created Resource Group. 
</p>
<br />

<p>
<img src="https://i.imgur.com/1oncBVH.png" height="80%" width="80%" alt=""/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Ge0tgVu.png" height="80%" width="80%" alt=""/>
</p>
<p>
2.) Use Remote Desktop to connect to your Windows 10 VM. Install Wireshark by searching 'download wireshark' in the web browser. Download the 'Windows Installer (64-bit)' from the Wireshark website. Run the installer and complete the installation steps. Open Wireshark (Ethernet) and filter for ICMP traffic only by typing 'ICMP' in the search bar. Go back to the Azure portal, and obtain VM2's private IP address from the overview tab. Attempt to ping it from the Windows 10 VM, and observe the requests and replies within Wireshark. Ping a public website (such as www.google.com) and observe the traffic in Wireshark. Initiate a non-stop ping from your Windows 10 VM to your Ubuntu VM. Go back to the Azure portal -> Network Security Groups -> VM2 -> Inbound security rules -> Add -> Source: Any -> Source port ranges: * -> Destination: Any -> Service: Custom -> Destination port ranges: * -> Protocol: ICMP -> Action: Deny -> Priority: 200 -> Any applicable name will work. Add this new rule and observe the results back in your Windows 10 VM. Once the rule has taken effect, the request will begin to time out in Wireshark. From here, you can re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using. This should allow the ping activity to resume in your Windows 10 VM. Stop the ping with 'CTRL+C'
</p>
<br />

<p>
<img src="https://i.imgur.com/4cV9CAI.png" height="80%" width="80%" alt=""/>
</p>
<p>
3.) Back in Wireshark, filter for SSH traffic only. From your Windows 10 VM, "SSH into" your Ubuntu VM via its private IP address <i>ssh labuser@10.0.0.5</i> (labuser in this case will be the username you used to create your VMs). Follow the prompts. Type commands (username, pwd, etc.) into the Linux SSH connection and observe SSH traffic spam in Wireshark. Exit the SSH connection by typing 'exit' and pressing Enter.
</p>
<br />

<p>
<img src="https://i.imgur.com/bZWY4oT.png" height="80%" width="80%" alt=""/>
</p>
<p>
4.) Back in Wireshark, filter for DHCP traffic only. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line <i>ipconfig /renew</i> and observe the DHCP traffic in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/uwPT12W.png" height="80%" width="80%" alt=""/>
</p>
<p>
5.) Back in Wireshark, filter for DNS traffic only (udp.port == 53). From your Windows 10 VM within a command line, use <i>nslookup</i> to see what google.com and disney.com's IP addresses are and observe the results in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/at6gdWu.png" height="80%" width="80%" alt=""/>
</p>
<p>
6.) Back in Wireshark, filter for RDP traffic only (tcp.port == 3389) and observe the traffic spam. The reason for this spam is because the RDP protocol is constantly showing you a live stream from one computer to another, therefore, traffic is always being transmitted.
</p>
<br />


![sebee](https://user-images.githubusercontent.com/119359052/213799053-302c5da8-886a-47b0-a6f2-4b5951fe1cbc.gif)



<p>
7.) From here, just close out your Remote Desktop Connection, and delete all resource groups running on Azure. You did it!
</p>
<br />
