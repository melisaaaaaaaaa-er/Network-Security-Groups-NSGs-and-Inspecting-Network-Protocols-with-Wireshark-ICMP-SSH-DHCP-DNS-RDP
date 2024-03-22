# Network Security Groups (NSGs) and Inspecting Network Protocols with Wireshark - ICMP, SSH, DHCP, DNS, RDP

<h3>Purpose</h3>

- Testing network connectivity between two virtual machines (VMs) and observing network protocols using Wireshark and PowerShell.

<h2>Deployment and Configuration Steps:</h2>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/16.png"/>

Locate the public IP address for the Windows 10 VM on the VM interface.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/17.png"/>

Open up the Remote Desktop Connection application and use the Windows 10 VM public IP to establish the remote connection session.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/18.png"/>

Enter the administrator account credentials you input during the VM setup process and click “OK”.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/19.png"/>

Select “Yes”.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/20.png"/>

The remote connection has established. We are now inside the Windows 10 VM created in Azure.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/21.png"/>

When prompted, select “Yes” to make the VM discoverable by other devices on the network.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/22.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/23.png"/>

Install Wireshark on the VM by going to the official Wireshark website through the browser (Internet Explorer). 

Download the “Windows x64 Installer” option and follow installation wizard. There’s no need to customize the installation process for this lab – stick with the pre-selected options.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/24.png"/>

On Azure, go to the Ubuntu linux VM interface and find the private IP address this time. This is needed for the network connectivity testing part of the lab.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/25.png"/>

When scanning with Wireshark, the flood of traffic going through a network at any given time is immediately seen. Protocols are color coded by default for easier identification, but still requires filtering for proper analysis as the quantity of traffic to sift through is massive.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/26.png"/>

Filtering is done by search for specific protocols using the search bar at the top of the interface. 

It’s also possible to filter by other criteria such as IP and MAC addresses.

This example will use ICMP (Internet Control Messaging Protocol) -  the protocol used by the ping command, typically used to test connectivity between two machines on a network.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/27.png"/>

Here are the ping packets sent to the Ubuntu linux VM (IP address 10.0.0.5) in purple.

Wireshark also allows the analysis of network traffic packets (i.e. we can see the message these packets are carrying) in the “Data” section of the information box. The specific message is shown on the left. In this case, it’s just the English alphabet.

Packet data is usually not visible in plain text like this due to encryption. Since ping doesn't send any valuable information, it doesn’t need to use encryption and we are able to see the data it is sending.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/28.png"/>

Input the command “ping 10.0.0.5 –t” in PowerShell to send ping requests indefinitely. 
This will be used to showcase the effects of Network Security Group functioning in Azure.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/29.png"/>

Network security groups act as the firewall for VMs on Azure.
Navigate to the “Network security group” service interface using the search bar.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/30.png"/>

Select “Ubuntu-linux-nsg” → “Inbound security rules” → “+ Add”.

Select “ICMP” under the Protocol section, and “Deny” under Action.

Give the rule a priority of 200. Priority is used to determine the order in which rules are applied. The lower the priority number (i.e. closer to 1) the higher priority it has, making it the one that is implemented. Priority has greater relevance when there are several rules covering the same protocol.

Give the rule a name and select “Add”.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/31.png"/>

Here is the new rule in the “Inbound security rules” interface blocking ping traffic for the Ubuntu linux VM. This prevents the Ubuntu VM from receiving ping requests.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/32.png"/>

The effects of this rule can be seen in the Windows 10 VM where ping requests are timing out (PowerShell) and no reply packets are coming in - only request packets are going out (Wireshark).

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/33.png"/>

Go back to the new rule and edit it to allow ICMP traffic. This will have the same effect as outright deleting the rule. Click “Save”.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/34.png"/>

Back on the Windows 10 VM, we can see that pinging the Ubuntu linux VM is working once again both on PowerShell and Wireshark.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/35.png"/>

The next protocol tested is SSH (Secure Shell) used to establish remote connectivity to machines through the command line.

Using the command “ssh username@IP_address” I connected to the Ubuntu linux VM.

The SSH traffic packets can be seen on Wireshark in purple.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/36.png"/>

Here is a screenshot where I perform some tasks on the Ubuntu VM through the remote SSH connection.
As long as the SSH connection is present, SSH packets will continue to show up in Wireshark until the connection is closed.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/37.png"/>
Here is the test for DHCP where the Windows 10 VM requests a new IP address from Azure’s DHCP server.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/38.png"/>

Here is the DNS protocol test where the Windows 10 VM is performing a name resolution for www.google.com.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/vms-network-activities-images/main/39.png"/>

The final network test for this lab is testing the Remote Desktop Protocol (RDP) which has been in use throughout the lab since we’re connected to the Windows 10 VM remotely.

Another way to filter for protocols on Wireshark is by inputting their respective port, as seen here (tcp.port == 3389). The port for RDP is 3389.
