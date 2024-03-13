# Network Security Groups (NSGs) and Inspecting Network Protocols with Wireshark - ICMP, SSH, DHCP, DNS, RDP

<h2>Deployment and Configuration Steps:</h2>

![16](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/a5b59c3b-e09d-4f09-b1f8-a7c1cd11fc3b)

Locate the public IP address for the Windows 10 VM on the VM interface.

#
![17](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/bd417b4d-9b5b-4649-a3cc-6b896da90929)

Open up the Remote Desktop Connection application and use the Windows 10 VM public IP to establish the remote connection session.

#
![18](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/6af6bf57-cd85-4415-afac-b9419d9f82d0)

Enter the administrator account credentials you input during the VM setup process and click “OK”.

#
![19](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/118b3313-f651-471f-b86b-b1a01dd9097d)

Select “Yes”.

#
![20](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/14c84ee6-b4f0-4900-ac91-b0794cca0ccc)

The remote connection has established. We are now inside the Windows 10 VM created in Azure.

#
![21](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/c3d15eac-408e-4c6a-94f9-b390fc7b78ca)

When prompted, select “Yes” to make the VM discoverable by other devices on the network.

#
![22](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/7f824be7-ee18-44a8-a9ff-23b746aab54e)

![23](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/0ead03f3-6113-4b20-87e4-4b4deaaf52c9)

Install Wireshark on the VM by going to the official Wireshark website through the browser (Internet Explorer). 

Download the “Windows x64 Installer” option and follow installation wizard. There’s no need to customize the installation process for this lab – stick with the pre-selected options.

#
![24](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/295a41d7-6992-446b-b389-b57f060f19ee)

On Azure, go to the Ubuntu linux VM interface and find the private IP address this time. This is needed for the network connectivity testing part of the lab.

#
![25](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/001e5bdf-997e-4d32-aeb2-5aaf2a00d406)

When scanning with Wireshark, the flood of traffic going through a network at any given time is immediately seen. Protocols are color coded by default for easier identification, but still requires filtering for proper analysis as the quantity of traffic to sift through is massive.

#
![26](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/c1a30271-a867-4be9-83de-7959bc499457)

Filtering is done by search for specific protocols using the search bar at the top of the interface. 

It’s also possible to filter by other criteria such as IP and MAC addresses.

This example will use ICMP (Internet Control Messaging Protocol) -  the protocol used by the ping command, typically used to test connectivity between two machines on a network.

#
![27](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/ddbcde07-bad1-44fb-95d5-4d0496cb33dd)

Here are the ping packets sent to the Ubuntu linux VM (IP address 10.0.0.5) in purple.

Wireshark also allows the analysis of network traffic packets (i.e. we can see the message these packets are carrying) in the “Data” section of the information box. The specific message is shown on the left. In this case, it’s just the English alphabet.

Packet data is usually not visible in plain text like this due to encryption. Since ping doesn't send any valuable information, it doesn’t need to use encryption and we are able to see the data it is sending.

#
![28](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/4ccdad0a-91f5-41cf-a037-e463cfdc0b6d)

Input the command “ping 10.0.0.5 –t” in PowerShell to send ping requests indefinitely. 
This will be used to showcase the effects of Network Security Group functioning in Azure.

#
![29](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/41f4982a-3fd0-46bf-935b-363cdc62c60c)

Network security groups act as the firewall for VMs on Azure.
Navigate to the “Network security group” service interface using the search bar.

#
![30](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/6b8d1e79-e369-4855-b2ff-f39c57acfd4d)

Select “Ubuntu-linux-nsg” → “Inbound security rules” → “+ Add”.

Select “ICMP” under the Protocol section, and “Deny” under Action.

Give the rule a priority of 200. Priority is used to determine the order in which rules are applied. The lower the priority number (i.e. closer to 1) the higher priority it has, making it the one that is implemented. Priority has greater relevance when there are several rules covering the same protocol.

Give the rule a name and select “Add”.

#
![31](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/ce769a95-f09d-4e28-9004-b15bd0f08682)

Here is the new rule in the “Inbound security rules” interface blocking ping traffic for the Ubuntu linux VM. This prevents the Ubuntu VM from receiving ping requests.

#
![32](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/8eaf973b-1fc3-4035-a67c-5e3bf8121062)

The effects of this rule can be seen in the Windows 10 VM where ping requests are timing out (PowerShell) and no reply packets are coming in - only request packets are going out (Wireshark).

#
![33](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/f10ba644-d7b8-4a4c-beee-fc2ef8628526)

Go back to the new rule and edit it to allow ICMP traffic. This will have the same effect as outright deleting the rule. Click “Save”.

#
![34](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/6181db47-0dbb-4ee6-95de-73abc8d0dfc1)

Back on the Windows 10 VM, we can see that pinging the Ubuntu linux VM is working once again both on PowerShell and Wireshark.

#
![35](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/29252f3e-2cbc-4a3b-8104-22aa6adfee36)

The next protocol tested is SSH (Secure Shell) used to establish remote connectivity to machines through the command line.

Using the command “ssh username@IP_address” I connected to the Ubuntu linux VM.

The SSH traffic packets can be seen on Wireshark in purple.

#
![36](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/104f2fd4-9296-4709-8ac6-04123478ff2b)

Here is a screenshot where I perform some tasks on the Ubuntu VM through the remote SSH connection.
As long as the SSH connection is present, SSH packets will continue to show up in Wireshark until the connection is closed.

#
![37](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/8f9b9bee-047b-4f03-ae99-b1fe49d6f503)
Here is the test for DHCP where the Windows 10 VM requests a new IP address from Azure’s DHCP server.

#
![38](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/45dbf546-b916-40c0-a491-d8acee028bfb)

Here is the DNS protocol test where the Windows 10 VM is performing a name resolution for www.google.com.

#
![39](https://github.com/melisa-er/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols-with-Wireshark-ICMP-SSH-DHCP-DNS-RDP/assets/157723219/345d6ded-40c5-4975-943f-958730446bcd)

The final network test for this lab is testing the Remote Desktop Protocol (RDP) which has been in use throughout the lab since we’re connected to the Windows 10 VM remotely.

Another way to filter for protocols on Wireshark is by inputting their respective port, as seen here (tcp.port == 3389). The port for RDP is 3389.
