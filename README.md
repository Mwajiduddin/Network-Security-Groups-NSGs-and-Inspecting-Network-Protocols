<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/nettrafi%20diagram.png" height="15%" width="30%" alt="Traffic Examination"/>
</p>

<h1>Introduction</h1>
In this tutorial we will inspect the different network traffic between two virtual machines using a network protocol analyzer called Wireshark. as well as experiment with Network Security Groups. <br />

<h2>Tutorial Guidelines</h2>
 
<h3>Step 1: Creating two different virtual machines</h3>

 First we start off by creating a Windows 10 virtual machine in Azure, this part of step was done in the previous "How to create a virtual machine in Microsoft Azure" tutorial which can found [here](https://github.com/Mwajiduddin/How-to-create-a-virtual-machine-in-Microsoft-Azure) but I'll do a quick rundown on how to make one here as well.
 
 Log into Microsoft Azure, name and create a Resource group by typing in "Resource groups" in the search box or click on the "Resource groups" icon. Here the resource group is named "RG-VMs" and the region is located at East US.
 <p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b1.png" height="40%" width="60%"/>
</p>

Then type in "virtual machines" in the search box or click on the "virtual machines" icon. Create a virtual machine with a Windows 10 Pro operating system, remember to select the same name and the same region of the resource group that you just made. Since we will be interacting with another VM, it's best to select a bigger size than the default so here I chose this VM to have 2 virtual CPUs with 16GB of RAM. Make and remember the username and password that you will log into of this VM, check the Licensing box at the lower left corner and click on "Review + create."

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b2.png" height="60%" width="75%"/>
</p>

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b3.png" height="60%" width="75%"/>
</p>

Now we will be creating our second virtual machine so same process as last time: type in "virtual machines" in the search box, name it, select the same name and region of the resource group you first made, select the same size as well and right next to "Image" select "Ubuntu Server 20.04 LTS - x64 Gen2 (free services eligible)."

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b4.png" height="60%" width="75%"/>
</p>

Under "Administrator Account" next to "Authentication type" select the "Password" option instead of "SSH public key." For simplicity sake, you can use the same username and password that you chose for the Windows 10 VM that you made and then click "Review + create."

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b05.png" height="60%" width="75%"/>
</p>

<h3>Step 2: Downloading and running Wireshark in Windows 10 VM</h3>
Once the VMs have been made, we are going to log into our Windows 10 VM by way of Remote Desktop Connection. To do this we first need the public IP address for the Windows 10 VM and this can be found by going into "Virtual machines" in Azure, selecting the Windows machine and copying the IP address. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b06.png" />
</p>

Then search and select "Remote Desktop Connection" in the Windows search bar. Paste the IP address where it says "Computer" and click Connect. 
<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b7.png" />
</p>

You will be prompted by a window asking for credentials (the same credentials that you made under "Adminstrator account") so click on "More choices" and then click on "Use a different account." Type in the username and password that you created and click on OK. Then you will be prompted by another window stating that "The identity of the remote computer cannot be verified. Do you want to connect anyway?" click Yes.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b8.png" />
</p>

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b9.png" />
</p>

As the virtual machine is booting you will see a screen about privacy settings for your device, just select no for each option and hit "Accept." Then on the right hand side there will be prompt about network discoverablity for this VM, select Yes.

Now that our Windows virtual machine is up and running we can observe its network traffic by way of a software called Wireshark. To do this we go into Google and type in "download Wireshark," click on the first link and select Windows Installer (64-bit). Click on "Open file" after the download is finished and install the program. 

Open up Wireshark by searching "Wireshark" in the Windows search bar. You can view live network traffic by selecting "Ethernet" and clicking on the blue icon on the top left corner.  

<h3>Step 3: Running different network protocols and observing its traffic using Wireshark</h3>
We can observe connectivity between our two virtual machines by pinging our Ubuntu VM from our Windows 10 VM. To do this we first need to filter our network traffic by typing in ICMP at the top bar (ICMP is the protocol used by ping) and you'll notice that there isn't any traffic. In order to ping to our Ubuntu VM we need to retrieve it private IP address which can be found in our Azure account and clicking on our Ubuntu VM in Virtual machines. After copying the private IP address, go back to your Windows VM, open Powershell from the Windows search bar and type in "ping (private IP address)" and press Enter. You'll notice that Wireshark will display traffic between two IP addresses under Source and Destination column, here 10.0.0.4 is the Windows VM and 10.0.0.5 is the Ubuntu VM thus proving connectivity between the VMs. 

We can also see connectivity between the Windows VM and a website for instance. To test this go into Powershell, type in ping and the website you would like to test connectivity to and hit Enter. Here you can see that 10.0.0.4 is the Windows VM and 172.253.115.136 is the website YouTube proving the VM connected to youtube.com.

We can set the Windows VM to ping to Ubuntu VM perpetually by typing in "ping (private IP address) -t" and hit Enter; notice Wireshark is constantly displaying network traffic between the virtual machines nonstop. While the Windows VM is pinging our Ubuntu VM, we can alter the Firewall rules of the Ubuntu VM to prevent ICMP traffic coming through thus preventing connectivity between the two VMs. Basically we are going to create a new firewall rule that will deny any incoming ICMP traffic. To do this, go into Azure, type in "Network Security Group" in the search bar (which is essentially the virtual machine's Firewall), select the Ubuntu VM network security group, click on "Inbound security rules" and click "+ Add." Here select "ICMP" under Protocol and "Deny" under Action. Change the Priority value above SSH's 300 so that the rule we made will be the first rule enact, here I've chose 200 as the rule's Priority value and click Add. And once you're done adding this rule, notice the network traffic in Wireshark and Powershell, it basically shows that the Windows VM is pinging the Ubuntu VM yet there is no connection between them. To allow connectivity again between the two VMs, just go back to the Inbound security rule, select "allow" and "Save" and you should notice correspondence between the virtual machines. Hit Ctrl + C in Powershell to stop pinging.


























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
