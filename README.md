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

In this tutorial, we are going to observe Network Security Protocols, while inspecting traffic between Azure Vitual Machines. We will be creating two Virtual Machines in Azure to get a greater sense for how these systems work. We will be creating one Virtual Machine running Windows 10 and one running Linux Ubuntu. So, start by going into Azure and create these two Virual Machines.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/6bc8286a-7250-4587-b397-b3cf96d4e3f2)


Now, we will open Remote Desktop from our command bar and input the public IP Address for the Virtual Machine that is running Windows 10. This will allow you to install WireShark (Packet Analysis Software). Be sure that you are on the Windows 10 Virtual Machine when doing so.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/e3a1aa0d-92ec-4143-8bc8-43b329f1c789)

When you install Wireshark, continue with the default settings and NpCap will pop up to be installed. Go ahead and install this will all of the default setting and Wireshark will be downloaded onto your Virtual Machine.

Open Wireshark -> Open Ethernet -> Click the blue fin under Files. After inputing the private IP Address of the Linux Vitual Machine, we are able to see the traffic between networks.

Now that Wireshark is set up and running, we can use the Ping Command to test the connectivity between machines. To do this filter ICMP traffic in Wireshark. We are also able to Ping  other IP Addresses or domain names (www.Amazon.com).

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/04620c0c-5fa3-4680-a709-c80a7761bb02)

We will now deny Ping request by Adding this rule to our Network Security Group in our Linux Vitural Machine in Azure.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/4d546fd0-13eb-4d72-8cb2-5ebc54a45a0d)

Here is an example of Wireshark and Powershell timed out after denying ICMP traffic.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/bfb0ff87-e9e4-4719-b267-cc3bad0d02ef)

We now will filter SSH traffic only in our Linux Virual Machine. This connection can be exited by typing "Exit" in the command bar and pressing enter.

We will also Filter for DHCP traffic only. To do this type "Ipconfig/renew" into the command line. We may now observe DHCP traffic in Wireshark because the Window 10 Machine's IP Address has been renewed. 

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/ee100e69-df28-42ab-bcff-4a0fb57ef274)

We are also able to filter RDP traffic only by typing in tcp.port== 3389 to the command bar. The RDP protocol is constantly sending packets over the Network, so we will see that traffic is constantly being sent.

![image](https://github.com/emodjeska/azure-network-protocols/assets/143763072/b6d5dcb9-0a81-41aa-ab93-57682cef2d8c)

That concludes today's tutorial. Hopefully we have a greater understand of Network protocols and how information is sent over the Network. Thank you for joining me.

