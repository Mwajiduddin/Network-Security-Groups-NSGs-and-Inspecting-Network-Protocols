<h1 align="center">Observing network traffic between two virtual machines in Azure using Wireshark</h1>


<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/nettrafi%20diagram.png" height="15%" width="30%" alt="Traffic Examination"/>
</p>

<h1>Introduction</h1>
In this tutorial we will utilize different command protocols in Powershell to observe network traffic between two virtual machines using a network protocol analyzer called Wireshark. We will also configure Network Security Groups (Azure Firewall) to change connectivity between the two VMs.    <br />

<h2>Tutorial Guidelines</h2>
 
<h3>Step 1: Creating two different virtual machines</h3>

 First we start off by creating a Windows 10 virtual machine in Azure, this part of step was done in the previous "How to create a virtual machine in Microsoft Azure" tutorial which can found [here](https://github.com/Mwajiduddin/How-to-create-a-virtual-machine-in-Microsoft-Azure) but I'll do a quick rundown on how to make one here as well.
 
 Log into Microsoft Azure, name and create a Resource group by typing in "Resource groups" in the search box or click on the "Resource groups" icon. Here the resource group is named "RG-VMs" and the region is located at East US.
 <p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b1.png" height="40%" width="60%"/>
</p>

Then type in "virtual machines" in the search box or click on the "virtual machines" icon. Create a virtual machine with a Windows 10 Pro operating system, remember to select the same name and the same region of the resource group that you just made. Since we will be interacting with another VM, it's best to select a bigger size than the default so here I chose this VM to have 2 virtual CPUs with 16GB of RAM. Make and remember the username and password that you will log into of this VM, check the Licensing box at the lower left corner, click on "Review + create" and after validation is complete click on "Create" at the bottom.

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

Under "Administrator Account" next to "Authentication type" select the "Password" option instead of "SSH public key." For simplicity sake, you can use the same username and password that you chose for the Windows 10 VM that you made, then click on "Review + create" and after validation click on "Create."

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

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b10.png" />
</p>

Now that our Windows virtual machine is up and running we can observe its network traffic by way of a software called Wireshark. To do this we go into Google and type in "download Wireshark," click on the first link and select Windows Installer (64-bit). Click on "Open file" after the download is finished and install the program. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b11.png" />
</p>

Open up Wireshark by searching "Wireshark" in the Windows search bar. You can view live network traffic by selecting "Ethernet" and clicking on the blue icon on the top left corner.  

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b12.png" />
</p>

<h3>Step 3: Running different network protocols and observing its traffic using Wireshark</h3>

<h2 align="center">Utilizing ICMP protocol</h2>


We can observe connectivity between our two virtual machines by pinging our Ubuntu VM from our Windows 10 VM. To do this we first need to filter our network traffic by typing in ICMP at the top bar (ICMP is the protocol used by ping) and you'll notice that there isn't any traffic displayed in Wireshark.


<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b13.png" height="70%" width="85%"/>
</p>

In order to ping to our Ubuntu VM we need to retrieve it private IP address which can be found in our Azure account and clicking on our Ubuntu VM in Virtual machines. After copying the private IP address, go back to your Windows VM, open Powershell from the Windows search bar and type in "ping (private IP address)" and press Enter. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b14.png" />
</p>

You'll notice that Wireshark will display traffic between two IP addresses under Source and Destination column, here 10.0.0.4 is the Windows VM and 10.0.0.5 is the Ubuntu VM thus proving connectivity between the VMs. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b15.png" />
</p>

We can also see connectivity between the Windows VM and a website for instance. To test this go into Powershell, type in ping and the website you would like to test connectivity to and hit Enter. Here you can see that 10.0.0.4 is the Windows VM and 172.253.115.136 is the website YouTube proving the VM connected to youtube.com.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b16.png" />
</p>

We can set the Windows VM to ping to Ubuntu VM perpetually by typing in "ping (private IP address) -t" and hit Enter; notice Wireshark is constantly displaying network traffic between the virtual machines nonstop. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b17.png" />
</p>

<h2 align="center">Utilizing Network Security Groups</h2>

While the Windows VM is pinging our Ubuntu VM, we can alter the Firewall rules of the Ubuntu VM to prevent ICMP traffic coming through thus preventing connectivity between the two VMs. Basically we are going to create a new firewall rule that will deny any incoming ICMP traffic. To do this, go into Azure, type in "Network Security Group" in the search bar (which is essentially the virtual machine's Firewall), select the Ubuntu VM network security group, click on "Inbound security rules" and click "+ Add." 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b18.png" />
</p>

Here select "ICMP" under Protocol and "Deny" under Action. Change the Priority value above SSH's 300 so that the rule we made will be the first rule enact, here I've chose 200 as the rule's Priority value and click Add.
And once you're done adding this rule, notice the network traffic in Wireshark and Powershell, it basically shows that the Windows VM is pinging the Ubuntu VM yet there is no connection between them.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b19.png" />
</p>

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b20.png" />
</p>

To allow connectivity again between the two VMs, just go back to the Inbound security rule, select "Allow" and "Save" and you should notice correspondence between the virtual machines. Hit Ctrl + C in Powershell to stop pinging.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b21.png" />
</p>

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b22.png" />
</p>

<h2 align="center">Utilizing SSH protocol</h2>

We can access Ubuntu VM from the Windows VM by the Powershell command line using SSH. In Wireshark, filter the traffic by typing in "ssh" in the top bar and click the green fin icon to refresh. Then in Powershell, type  in "ssh (Ubuntu VM username)@(private IP address)" and hit Enter.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b23.png" />
</p>

You will receive a prompt if you want to continue so type "yes." Then you will need to enter the password that you made for your Ubuntu VM under "Administrator account" and when you type in your password you'll notice that there's no visible text appearing, there is nothing wrong with Powershell you just need to type it in correctly. After you've type in your password and hit Enter, you successfully logged in Ubuntu's VM marked by green text virtual machine name.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b24.png" />
</p>

You can list the directories (folders) of Ubuntu VM by typing in "ls -lasth" and press Enter and as you do you will notice the traffic displayed in Wireshark. You can exit out of Ubuntu VM by typing in "exit" and pressing Enter.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b25.png" />
</p>


<h2 align="center">Utilizing RDP protocol</h2>

We can observe RDP (Remote Desktop Protocol) traffic by its protocol and port number in Wireshark. We can do this by filter RDP traffic by typing "tcp.port == 3389" in the top bar and you will see a continuous display of traffic.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b28.png" />
</p>

<h2 align="center">Utilizing DHCP protocol</h2>

We can observe DHCP traffic in Wireshark by re-issuing our IP address of our Windows VM via the ipconfig /renew command in Powershell. First filter DHCP traffic by typing "dhcp" in the top bar and refresh. Then type in "ipconfig /renew" in the Powershell and observe the traffic in Wireshark. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b26.png" />
</p>


<h2 align="center">Utilizing DNS protocol</h2>


We can also observe DNS traffic by the nslookup command in Powershell, this command gives us the IP address of a website. Filter traffic by DNS in Wireshark then type in "nslookup (your choice of website)" into Powershell and observe the network traffic in Wireshark. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b27.png" />
</p>

We're almost finished, all we have to do now is to delete our resource groups in Azure so that it won't charge us more for using its services unnecessarily. So go into Azure, type in "Resource groups", click on the Resource group(s) shown, select "Delete resource group", copy the resource group's name and paste it below and then hit "Delete." Do this for each resource group and that's it for this tutorial.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b29.png" />
</p>
