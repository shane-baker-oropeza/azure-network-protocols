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

- Create virtual machines
- Configure firewall (Network Security Group)
- Observe SSH traffic
- Observe DHCP traffic
- Observe DNS traffic

<h2>Part 1: Azure Environment Setup</h2>

<h3>Step 1: Create a Resource Group</h3>

- Log in to the Azure Portal. 
- Search for "Resource Groups" and create a new one (RG-Network-Activities). This acts as a container for all your project resources.

<br />
<p>
<img width="524" height="569" alt="Screenshot 2026-01-23 004256" src="https://github.com/user-attachments/assets/4d99ac89-94ba-42fa-8d36-9dd002b5df5b" />

</p>
<br />

<p>
<img width="1094" height="454" alt="Screenshot 2026-01-23 004308" src="https://github.com/user-attachments/assets/58bfc7e8-6418-4c4f-9769-2497fcc252f1" />

</p>
<br />

<p>
<img width="732" height="858" alt="Screenshot 2026-01-23 004337" src="https://github.com/user-attachments/assets/4627d747-154a-4783-8248-fcdad932ea44" />

</p>
<br />

<p>
<img width="539" height="862" alt="Screenshot 2026-01-23 004354" src="https://github.com/user-attachments/assets/84e5a989-5274-4281-b6dc-fb64d91856e4" />

</p>
<br />

<h3>Step 2: Create the Windows 10 Virtual Machine</h3>

- Create a new VM.
- Select your Resource Group (RG-Network-Activities)
- Name it windows-vm and choose Windows 10.
- Under the "Networking" tab, allow Azure to create a new Virtual Network (lab5-vnet) and Subnet.

<br />
<p>
<img width="527" height="569" alt="Screenshot 2026-01-23 004420" src="https://github.com/user-attachments/assets/2b87d0a3-deb9-4f37-83ae-09230615b03d" />

</p>
<br />

<p>
<img width="677" height="674" alt="Screenshot 2026-01-23 004428" src="https://github.com/user-attachments/assets/fa3c5fc6-5429-4aa2-af2d-c786e7b9766c" />

</p>
<br />

<p>
<img width="834" height="862" alt="Screenshot 2026-01-23 074819" src="https://github.com/user-attachments/assets/2b89fe3d-84ae-423a-8736-0d36edab6d4b" />

</p>
<br />

<p>
<img width="787" height="759" alt="Screenshot 2026-01-23 074839" src="https://github.com/user-attachments/assets/e95097cd-5590-4a2f-99ed-e548abbca25b" />

</p>
<br />

<p>
<img width="819" height="863" alt="Screenshot 2026-01-23 074851" src="https://github.com/user-attachments/assets/49cb9f55-c84f-40be-abc4-2682e7a3c4b5" />

</p>
<br />

<p>
<img width="836" height="772" alt="Screenshot 2026-01-23 074936" src="https://github.com/user-attachments/assets/5bbc676f-e2b1-415f-a10c-47fa8b068231" />

</p>
<br />

<p>
<img width="829" height="862" alt="Screenshot 2026-01-23 074947" src="https://github.com/user-attachments/assets/6efdf66b-29e4-4753-883a-5636a92dc24d" />

</p>
<br />

<p>
<img width="782" height="882" alt="Screenshot 2026-01-23 075118" src="https://github.com/user-attachments/assets/ea693077-4787-4336-bc43-117c3cb03144" />

</p>
<br />

<p>
<img width="854" height="887" alt="Screenshot 2026-01-23 075143" src="https://github.com/user-attachments/assets/4a2820f4-e049-4f76-87d3-9d195fd4b536" />

</p>
<br />

<p>
<img width="787" height="878" alt="Screenshot 2026-01-23 075200" src="https://github.com/user-attachments/assets/8105f43b-2253-46a6-92e8-bd12a7b67479" />

</p>
<br />

<h3>Step 3: Create the Linux (Ubuntu) Virtual Machine</h3>

- Create another VM.
- Select the same Resource Group and the same Virtual Network created in Step 2.
- Name it linux-vm and choose Ubuntu.
- Use "Password" for the authentication type.
<br />

<p>
<img width="693" height="667" alt="Screenshot 2026-01-23 075329" src="https://github.com/user-attachments/assets/14fd76c2-faf0-4873-a07e-94bd93f5daf5" />

</p>
<br />

<p>
<img width="824" height="871" alt="Screenshot 2026-01-23 075406" src="https://github.com/user-attachments/assets/f71a4aed-a777-423f-82cb-3c10d76e1120" />

</p>
<br />

<p>
<img width="815" height="880" alt="Screenshot 2026-01-23 075425" src="https://github.com/user-attachments/assets/93975a43-1391-4822-bac7-d227b3e9ea1c" />

</p>
<br />

<p>
<img width="863" height="897" alt="Screenshot 2026-01-23 075441" src="https://github.com/user-attachments/assets/63821bb6-3cea-4549-a6ed-7e5379da4100" />

</p>
<br />

<p>
<img width="827" height="883" alt="Screenshot 2026-01-23 075453" src="https://github.com/user-attachments/assets/54fbf2be-8870-420d-af19-32dd9abb6e14" />

</p>
<br />

<p>
<img width="937" height="884" alt="Screenshot 2026-01-23 075516" src="https://github.com/user-attachments/assets/ee18c3fc-d428-4c03-be23-65d74d0ffa6c" />

</p>
<br />

<p>
<img width="780" height="887" alt="Screenshot 2026-01-23 075534" src="https://github.com/user-attachments/assets/1cdf3706-d8cc-4420-ac1c-40e8f0f9b99d" />

</p>
<br />

<h3>Step 4: Verify Connectivity</h3>

- Navigate to the "Virtual Networks" tab to ensure both VMs are listed under the same internal subnet.
<br />

<p>
<img width="2038" height="616" alt="Screenshot 2026-01-23 075648" src="https://github.com/user-attachments/assets/db192370-6c4e-4916-9b48-5cf0452f86f8" />

</p>
<br />

<p>
<img width="1677" height="844" alt="Screenshot 2026-01-23 075705" src="https://github.com/user-attachments/assets/8dc0d549-c8f7-4dfc-8a35-be1d02a8afa1" />

</p>
<br />

<p>
<img width="1766" height="839" alt="Screenshot 2026-01-23 075725" src="https://github.com/user-attachments/assets/3116fdd1-50aa-4e1c-a278-5c8523c2f4d3" />

</p>
<br />

<h2>Part 2: Observing ICMP Traffic</h2>

<h3>Step 5: Connect via Remote Desktop (RDP)</h3>

- Open Microsoft Remote Desktop. 
- Use the Public IP of your windows-vm to log in to the desktop environment.
<br />

<p>
<img width="2038" height="616" alt="Screenshot 2026-01-23 075648" src="https://github.com/user-attachments/assets/da5a33ff-5838-43eb-a467-9948a5051ff7" />

</p>
<br />

<p>
<img width="409" height="488" alt="Screenshot 2026-01-23 075800" src="https://github.com/user-attachments/assets/b7fe99e3-7aa5-4bb7-bf4c-e05667ba8e5b" />

</p>
<br />

<p>
<img width="462" height="418" alt="Screenshot 2026-01-23 075815" src="https://github.com/user-attachments/assets/4b6256af-c2ce-4f23-9504-f5371b1eede5" />

</p>
<br />

<h3>Step 6: Install and Configure Wireshark</h3>

- Inside the Windows VM, download and install Wireshark. 
- Open it, select the Ethernet adapter, and start a packet capture.
<br />

<p>
<img width="650" height="359" alt="Screenshot 2026-01-23 080113" src="https://github.com/user-attachments/assets/9c729d83-d112-4753-8346-e794a85c58b1" />

</p>
<br />

<p>
<img width="855" height="776" alt="Screenshot 2026-01-23 080125" src="https://github.com/user-attachments/assets/c0f0aa93-82d4-4c0d-87ef-ed91430e4f16" />

</p>
<br />

<p>
<img width="1063" height="688" alt="Screenshot 2026-01-23 080135" src="https://github.com/user-attachments/assets/b7a6af65-7945-4913-8d14-7adf341d6f46" />

</p>
<br />

<p>
<img width="661" height="684" alt="Screenshot 2026-01-23 080514" src="https://github.com/user-attachments/assets/8a61b751-d970-4271-b356-ebd81eb528d6" />

</p>
<br />

<p>
<img width="977" height="583" alt="Screenshot 2026-01-23 080547" src="https://github.com/user-attachments/assets/8509a320-f625-45e0-9aa4-a16ac0ad99ff" />

</p>
<br />

<p>
<img width="1376" height="735" alt="Screenshot 2026-01-23 080604" src="https://github.com/user-attachments/assets/81d57b26-e567-4647-a64d-d2a32fab3b9b" />

</p>
<br />

<h3>Step 7: Filter and Observe ICMP (Ping)</h3>

- In the Wireshark filter bar, type icmp. 
- Open PowerShell and ping your Ubuntu VM's Private IP (ping 172.17.1.4).
<br />

<p>
<img width="1378" height="734" alt="Screenshot 2026-01-23 080619" src="https://github.com/user-attachments/assets/d6ed22e7-8787-457f-99b0-6643b71c6b29" />

</p>
<br />

<p>
<img width="1687" height="770" alt="Screenshot 2026-01-23 080740" src="https://github.com/user-attachments/assets/5eca8ed5-7938-45e0-a7c5-ecd0246e1f69" />

</p>
<br />

<p>
<img width="803" height="694" alt="Screenshot 2026-01-23 080923" src="https://github.com/user-attachments/assets/de487ab2-386d-4961-94e8-20efe8ea6604" />

</p>
<br />

<p>
<img width="863" height="734" alt="Screenshot 2026-01-23 081045" src="https://github.com/user-attachments/assets/58caccc0-13ed-408e-a69c-876c0f6092e4" />

</p>
<br />

<p>
<img width="865" height="737" alt="Screenshot 2026-01-23 081104" src="https://github.com/user-attachments/assets/4254e69c-fc10-4c85-9e58-d62ddfda2ff3" />

</p>
<br />

<p>
<img width="1093" height="735" alt="Screenshot 2026-01-23 081110" src="https://github.com/user-attachments/assets/ac9b2acf-4a10-4dbe-b59a-1ec70b919a8d" />

</p>
<br />

<h2>Part 3: Firewall Configuration & Protocol Analysis</h2>
<h3>Step 8: Configure the Network Security Group (NSG)</h3>

- Initiate a "perpetual ping" from PowerShell: ping 172.17.1.4 -t.
- In the Azure Portal, go to the Ubuntu VM's Networking settings and add an Inbound Port Rule to Deny ICMP.
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Step 9: Observe Firewall Impact</h3>

- Return to the Windows VM.
- Observe Wireshark showing "Request Timed Out" as the firewall blocks the traffic.
- Re-enable the traffic in Azure to see the pings resume.
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Step 10: Observe SSH Traffic</h3>

- In Wireshark, change the filter to ssh.
- In PowerShell, connect to the Linux VM: ssh labuser@<Private-IP>.
- Type commands into the Linux terminal.
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Step 11: Observe DHCP Traffic</h3>

- In Wireshark, filter for dhcp (or bootp). 
- In an Administrative PowerShell window, run ipconfig /renew.
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Step 12: Observe DNS Traffic</h3>

- In Wireshark, filter for dns. 
- In PowerShell, run nslookup www.google.com.
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Step 13: Observe RDP Traffic</h3>

- In Wireshark, filter for tcp.port == 3389.
- Notice the constant stream of traffic.

<h3>Analysis:</h3>

**Because RDP is a live-stream of the desktop interface, it must constantly send data to update your screen, resulting in "non-stop" traffic.**
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

