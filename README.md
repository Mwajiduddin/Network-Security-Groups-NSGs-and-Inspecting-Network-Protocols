<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

## Operating Systems Used

- Windows 10 (21H2)
- Linux Ubuntu Server 20.04

## High-Level Steps

- Create Resources
- Install Wireshark
- Observe Differing Network Protocols

## Actions and Observations


### Step 1 Install Resources
The first step will be creating a Resource group that will house our two Virtual Machines to observe the traffic being sent between the two machines. To Create the Resource Group you can do a quick search for `Research Group ` at the top of Azure or you can select `Create a Resource` and then choose to create the Resource group from the Azure Market Place. 

<p align="center">
  <img src="https://i.imgur.com/GKUzeBu.png" height="70%" width="70%" alt="search resource group" /> 
  </p>
  
After `Research Group` is entered and the results are returned, you can now select the `+ Create` button. This will begin the process of creating the resource group that will have eaach of our resources such as the virtual machines.  
<p align="center">
<img src="https://i.imgur.com/Tqk3vLR.png" height="80%" width="80%" alt="search and create resource group" />
</p> 

You will select the `subscription name`, enter your custom created `resource group` (here enter RG-LAB-2) name and select the preferred `region` that is the nearest to you that would assist saving on cost. As you are creating the resource group, you will see the option to create  tags as well; however, in this lab, the tag is not needed.

<p align="center">
<img src="https://i.imgur.com/Kwqp98O.png" height="60%" width="60%" alt="create resource group name" />
  </p>

The two virtual machines that we create will allow us to send traffic between the two machines. You can name the Virtual Machine whatever you prefer to name them and can be easy to remember. 

The first virtual machine that we will create will be Windows Operating System and will be named VM1, so we can do a quick search at the top of Azure for `Virtual Machine` then select Virtual machines from the search results. 

Once that is done, we can then choose to `+ Create` from the top left or at the center of the page. You will then select the `create a virtual machine hosted by azure` option. 

<p align="center">
 <img src="https://i.imgur.com/zzBqzpk.png" height="80%" width="80%" alt="create virtual machine" />
 </p> 

Below you will select the `subscription`, the same `resource group` (RG-LAB-2), name the `virtual machnine` (VM1), select the exact same `Region` as the resource group that was selected previously and the `windows image` then the others that are pictured below. 
<p align="center">
  <img src="https://i.imgur.com/oddn03a.png" height="70%" width="70%" alt="create virtual machine name" />
  </p>

The remaining portion of the basics section for the virtual machine consist of the virtual machine size, the username, password, inbound port rules and licensing. We allow port 3389 so that we can later remote desktop into the virtual machine. 

   >**Note**: Be sure to select the check box for licensing otherwise you will be prompting with an error message when the virtual machine is validation at time of creation.

<p align="center">
 <img src="https://i.imgur.com/U0QyvqS.png" height="70%" width="70%" alt="create size of windows vm"/>
  </p>
  
The networking section of creating the virtual machine will be set to the defaults and reflect as follows: 

<p align="center">
  <img src="https://i.imgur.com/f3LEt0R.png" height="70%" width="70%" alt="create networking portion of windows virtual machine"/>
  </p>

The other settings after Networking will be left to defaut and we can now select to `Review + Create` then review all of the details that we have selected for this particular virtual machine. Once that is done and everything looks good, we can go ahead and press the `Create` button.

<p align="center">
  <img src="https://i.imgur.com/6IAizlV.png" height="70%" width="70%" alt="review and create virtual machine"/>
  </p>
  
Now, we can create Virtual Machine 2 (VM2) that will have Linux Ubuntu Server image (pictured below) and will use password instead of ssh public key for authentication for remote access. Leaving the remaining sections (Networking, Management, Monitoring, Advanced and Tags) as their defaults so that we can simply `Review + Create` in a similar process that we saw previously while creating the Windows virtual machine. 

<p align="center">
  <img src="https://i.imgur.com/Dbv0TnB.png" height="70%" width="70%" alt="create linux virtual machine name" />
  </p>
  
 <p align="center">
  <img src="https://i.imgur.com/lccbqqo.png" height="70%" width="70%" alt="create linux virtual machine name" />
  </p>

We created two Virtual Machines (pictured below) of differing Operating Systems (Windows 10 21H2 & Linux Ubuntu Server 20.04) that will be used for Remote Deskop and to observe network traffic between the two devices. 


<p align="center">
<img src="https://i.imgur.com/BifIhxG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

### Step 2 Install Wireshark

A quick search for `"remote desktop connection"` will allow the us to access the VM. Here we will be entering the details of the public IP address for VM1 (Windows 10 21H2) to install Wireshark (packet analysis software) instead of using our local machine. (below pictured search of remote desktop and the result to enter IP address)

<p align="center">
<img src="https://i.imgur.com/PEeQyYV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

On the virtual machine with Windows 10 Pro, download Wireshark (Windows Installer 64-bit) and continue with all the default options.

<p align="center">
  <img src="https://i.imgur.com/xUswYty.png" height="70%" width="70%" alt="wireshark installer"/>
  </p>

Npcap will pop up to install, go ahead and install that with defaults and Wireshark will continue to install after.

After Wireshark has been completely installed, you can do a quick search at the bottom left of the windows Virtual Machine for `Wireshark` to open Wireshark. 

<p align="center">
  <img src="https://i.imgur.com/w9KdaQ3.png" height="70%" width="70%" alt="wirshark windows search"/>
  </p>
  
Open Wirehsark in the VM, click Ethernet and then click the blue fin at the top left under 'File' to begin capturing packets. Notice all the traffic already happening that happens in the background.

<p align="center">
  <img src="https://i.imgur.com/kc6biKy.png" height="70%" width="70%" alt="wireshark packet capture beginning"/>
  </p>
  
<p align="center">
  <img src="https://i.imgur.com/nV8wqOE.png" height="70%" width="70%" alt="wireshark packet capture background" />
  </p>

<br />

### Step 3 Observe Differing Network Protocols
After retrieving the private IP address from VM2 (Linux Ubutu Server 20.04) we can now ping that private IP address from VM1 (Windows 10 21H2) that we've used to remote into. We can use the ping command to test the connection between machines for connectivity. 

So we can now view the traffic travel from VM1 to VM2 by filtering the ICMP packets in Wireshark. We can also ping other IP address or a domain names (www.google.com). The filtered traffic (ICMP) in Wireshark that is requested and its corresponding reply is shown below in Wireshark is pictured (left) and Powershell (right):

<p align="center">
<img src="https://i.imgur.com/SriHAg2.png" height="80%" width="80%" alt="Wireshark and Powershell ICMP"/>
</p>

<br />

If we want to deny the ping request we can add this rule to our Network Security Group inside the Virtual Machine and once we've added this rule to VM2, we can see that the traffic times out in PowerShell along with Wireshark longer displaying a reply to this request. 

Wireshark and PowerShell timed out after adjusting to deny icmp (ping) traffic in the network security group inbound rules. The ping request is no longer being received but simply being timed out and is reflected below. 

In Azure Portal search for `Network Security Group` and click on the VM that has Linux Ubuntu Server

From there, click Inbound security rules, and click `+ Add.` Look for ICMP under the protocol radio buttons and make sure it is ticked. Under Action check Deny. For priority set it before 300 just so we can have this rule take place before any other rule.

   >**Note**: The lower the number, the higher the priority.
 
  <p align="center">
  <img src="https://i.imgur.com/mTtkBrg.png" height="80%" width="80%" alt="ping traffic"/>
  </p>
  <br/>
  
Once this rule is created, go back to Powershell and notice it will say Request timed out, and observe in wireshark how only requests are being shown.
  
  <p align="center">
<img src="https://i.imgur.com/8epnq3H.png" height="80%" width="80%" alt="icmp traffic deny"/>
</p>
</br>

To re-enable this rule, we can return back to the network security group to simply delete the rule or we can select the rule and allow the rule again. 

<p align="center">
  <img src="https://i.imgur.com/cPPjlpm.png" height="70%" width="70%" alt="deleter network group rule" /> 
  </p>

In wireshark change the filter to SSH or tcp.port == 22, then in Powershell type the login details for the Linux Ubuntu Server (using `"ssh username@ip address"` its private IP address).

Then type `yes` and it will ask for the password. Take note that as you are typing the password it will not show up in the terminal. When we use commands such as touch, pwd (print working directory) or ls (list), into the linux SSH that was used to connect. SSH traffic is observed spamming in WireShark. The SSH connection can be exited, by typing `exit` and pressing [Enter].

<p align="center">
<img src="https://i.imgur.com/DpJdcZl.png" height="70%" width="70%" alt="ssh traffic"/>
 </p> 
</br>

 We can filter in Wireshark for "DHCP traffic only". From VM1 (Windows 10 21H2), a new IP address was issued from the command line (ipconfig /renew). Now DHCP traffic can be observed in WireShark.
<p align="center">
<img src="https://i.imgur.com/GLm7cMT.png" height="80%" width="80%" alt="dhcp traffic"/>
</p>

DNS (Phonebook of the Internet) Traffic UDP Port 53

In Wireshark, filter to DNS traffic and click refresh to clear any traffic.

In Powershell, type in nslookup www.disney.com (this is basically asking what Disney's ip addresses are)

<p align="center">
  <img src="https://i.imgur.com/rQXcAol.png" height="80%" width="80%" alt="dns traffic"/>
  </p>

Using nslookup to see what is Disney's ip address

</br>
In Wireshark, we can filter for RDP traffic only (tcp.port == 3389) because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefore traffic is consistently being transmitted.

<p align="center">
<img src="https://i.imgur.com/eDKBXV1.png" height="70%" width="70%" alt="tcp 3389"/>
</p>

Now that we are done with this lab, we can delete the resource group by doing a search for `Resource Group` and then select the Resource Group that we created that contains the two Virtual Machines (Windows 10 Pro & Linux Ubuntu Server).

Once the Resource Group is selected, you will enter the Resource Group name and then choose to `Delete` at the top of the page and then the final delete button at the bottom of the page.

<p align="center">
  <img src="https://i.imgur.com/MgNl9rw.png" height="70%" width="70%" alt="delete resource group"/> 
  </p>

|Terms | Descriptions|
|-------|------------|
|Subscription| Subscription is a logical container used to provision related business or technical resources in Azure. It holds the details of all your resources like virtual machines (VMs), databases, and more. 
|Resource Groups| Resource Groups (similar to a file system) logical collections of virtual machines, storage accounts, virtual networks, web apps, databases, and/or database servers.  
|Virtual Machines (VM)| Virtual Machines (VM) allow you to more easily scale your applications by adding more physical or virtual servers to distribute the workload across multiple VMs.
|Remote Desktop| Remote desktop allows the user to connect to a computer in another location, see that computer's desktop and interact with it as if it were local.
|Tags| Tags are metadata elements that you apply to your Azure resources. They're key-value pairs that help you identify resources based on settings that are relevant to your organization
|Region| Each Azure region features datacenters deployed within a latency-defined perimeter. They're connected through a dedicated regional low-latency network. This design ensures that Azure services within any region offer the best possible performance and security.
|DHCP| DHCP (Dynamic Host Configuration Protocol) is a network management protocol used on Internet Protocol networks for automatically assigning IP addresses and other communication parameters to devices connected to the network using a client–server architecture.
|DNS| DNS or the Domain Name System, translates human readable domain names (for example, www.amazon.com) to machine readable IP addresses (for example, 192.0.2.44).

<p align="center"><i><b>🌤️"Learn something new, or a new way of approaching something old because there are a few skills that are as valuable as the art of learning.”🌤️</p></i></b>
