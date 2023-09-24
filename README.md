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

<h2>Actions and Observations</h2>

In this tutorial, we are going to learn about network security protocols and observe Network Security Protocols, while inspecting traffic between Azure Vitual Machines.

We will be creating two Virtual Machines in Azure to get a greater sense for how these systems work. We will be creating one Virtual Machine running Windows 10 and one running Linux Ubuntu. So, start by going into Azure and create these two Virual Machines. When you are done setting them up, be sure to use the same region and Virtual Network.

Now, we will open our Remote Desktop connection and input the public IP Address for the Virtual Machine that is running Windows 10. This will allow you to install WireShark (Packet Analysis Software). Be sure that you are on the Windows 10 Virtual Machine when doing so.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/a9cd5dc8-7bc9-48db-9dae-8167861830bb)

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/4d5550b4-790a-43fe-88fb-12eb2e646c27)


Now, we are going to install wireshark. When you install Wireshark, go ahead and install this with all of the default setting and Wireshark will be downloaded onto your Virtual Machine.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/d68475b9-b02b-4da4-b354-0e6d45476878)

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/56bf91b8-0efb-40fa-9e5f-32b8f422191c)

Open Wireshark -> Open Ethernet -> Click the blue fin under Files. After doing this you will be able to see all the traffic being next over the network.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/33ee0ab7-c41f-40cd-9e47-16399021f7bf)

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/a0b32583-eaff-4f71-a251-c18db8d00388)

Now, we are going to filter network traffic by ICMP (Internet Control messaging protcol). This is the protcol that ping uses. So now that traffic is being filtered by ICMP, we will only see Ping commands that are being sent over the network.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/f8996e21-035f-4e77-93dc-a38bb4e09297)

Then, we are going to send a ping command over the network to the second Virtual Machine that you made. To do this, locate the Private IP Address of the second Virtual Machine. After that, open up powershell by searching for it in the start menu. Type "ping" (Private IP Address) and press enter, to test the connectivity between the Virtual Machines.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/cbfd585b-cfd6-4a86-b743-f148bf568144)

After sending the Ping request you will notice the ICMP traffic being sent on wireshark.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/204be19d-a80f-4130-a9d4-2ddff2cbe389)

Next, we will type www.google.com on powershell, and you will see the see the traffic being sent over the network.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/a55b4724-426c-4c39-85f7-ea9422462150)

Go ahead and and ping your other Virtual Machine constantly, by typing ping (private IP address) -T into powershell. This will consistently send ICMP traffic over the network.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/1bbcc694-ce41-49ae-9584-3afcc78653f1)

We will now deny Ping request by Adding this rule to our Network Security Group in our Linux Vitural Machine in Azure. To do so, first go back into your local desktop and go into the Azure portal. Search Network Security Groups -> click on the VM running Linux -> click inbound security rules -> click add -> add a new security rule that denies ICMP traffic.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/36ef2533-d26d-4892-9c4e-62b2dee913c1)

Here is an example of Wireshark and Powershell timed out after denying ICMP traffic.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/e1cbaf55-69d5-4a36-ab39-be09fe9504b3)

Go back into the newtwork security groups for the Linux VM and set the rule you just created to allow ICMP traffic, instead of deny ICMP traffic. Go back into the Window VM and observe the ICMP traffic being sent over the network. After you observe the traffic type control C into Powershell, and it will stop all the ping commands from being sent.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/2d69e391-974a-4064-8ca7-1c945871c980)

We now will observe SSH traffic. SSH (Secure Shell Protocol) allows you to access other computers command lines remotely. To do this we will start by filtering for SSH traffic in Wireshark. Then we are going to go into powershell on the Windows VM and ping (LinuxVMUsername)@10.0.0.5., after you enter your password, you will establish an SSH connection to the Linux VM. This will allow you access to the command line of the Linux VM. Type "pwd", a linux command, into Powershell and you will see the traffic being sent on Wireshark. After that, enter "exit" in the the SSH command line to close the connection.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/ae2c7072-4d5e-4ca3-a50b-ff74820d3832)

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/baea0a77-65cb-426c-8242-b7982cc5ae16)

We will also observe DHCP traffic in Wireshark. To do this filter for DHCP in Wireshark. DHCP (Dynamic Host Configuration Protcol) assigns a new IP address to a PC if it does not already have one. To observe the traffic, type ipconfig /renew into Powershell to reset the system, this means that the DHCP protol will automatically assign it a new IP address.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/9d1c4a6f-60cd-48c4-9cf8-038cd3bbe935)

Now, we are going to observe DNS traffic. DNS (Domain Name System) converts IP Addresses into human readable terms. Start by filtering for DHS traffic in Wireshark. Type nslookup www.google.com to ask DNS what google's IP Address is. You will see the traffic being sent in Wireshark.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/1c897164-1000-4983-b83d-ac85bf0cfce1)

Also, We are able to observe RDP traffic. In Wirshark, filter for RDP traffic. The RDP (Remote Desktop Protocol) is constantly sending packets over the Network, so we will see that traffic is constantly being sent.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/3d7ca055-b5c9-4a8e-896c-09c76dbf1832)

That concludes today's tutorial.Together we observed traffic being sent over two self created Vitual Machines, used Powershell, we utilized network security protocols and we observed ICMP, SSH, DHCP, DNS, and RDP traffic on Wireshark (Packet Analysis Software). Hopefully we have a greater understand of Network protocols and how information is sent over the Network. 

Thank you for joining me.

