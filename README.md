# Active Directory Deployment and Bulk User Creation using PowerShell

**YouTube Demonstration – coming soon!**

## Description

In this lab we're to walk through how to create an Active Directory home lab Environment using Oracle Virtual Box. Configuring and running this lab will definitely help develop your understanding of how active directory and windows networking works, so I'd highly recommend running through it a couple times and then eventually try to build it on your without using this tutorial. This environment we're building is a pretty basic Windows networking environment with Active Directory, a small number of networking services and is common in many of today’s schools and organizations. So sit back, relax or better yet...copy what I'm doing. It will help you if you're trying to get into IT or wanting to learn more about Virtual Machines and Active Directory. Enjoy!


## Languages and Programs Used

-   **PowerShell**
-   **Oracle VirtualBox**

## Operating Systems

-   **Windows 10 ISO**
-   **Windows Server 2019 ISO**

## Download Links
<ul>
<li>
<p><span class="yt-core-attributed-string--link-inherit-color" dir="auto"><strong>Oracle VirtualBox</strong>:&nbsp;<a href="https://www.virtualbox.org/wiki/Downloads">https://www.virtualbox.org/wiki/Downloads</a></span></p>
</li>
<li>
<p><span class="yt-core-attributed-string--link-inherit-color" dir="auto"><strong>Server 2019 ISO</strong>:&nbsp;<a href="https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019">https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019</a></span></p>
</li>
<li>
<p><span class="yt-core-attributed-string--link-inherit-color" dir="auto"><strong>Windows 10 ISO</strong>:&nbsp;<a href="https://www.microsoft.com/en-us/software-download/windows10">https://www.microsoft.com/en-us/software-download/windows10</a></span></p>
</li>
<li><span class="yt-core-attributed-string--link-inherit-color" dir="auto">You can find the PowerShell script files right here in this repository.</span></li>
</ul>

## Network Diagram:
<p align="center">
<br/>
<img src="https://i.imgur.com/x12bzqc.png" height="90%" width="90%" alt="Microsoft Azure Virtual Machine"/>
<br /></p>
<br />


Just to give a high-level overview of what we're going to do in this lab, I’ll have you first refer to the network diagram so that you can look at to get an idea of what’s accomplished in this tutorial.
<br />
<br />

<h2><strong>This tutorial is broken down into 10 steps:</strong>&nbsp;</h2>
<ol>
<li>The first thing we're going to do is download and install Oracle VirtualBox, which is what we're going to use to run our virtual machines.<br /><br /></li>
<li>After that's installed, we're going to download a Windows 10 ISO and a Server 2019 ISO that we're going to use to install the two operating systems on two separate virtual machines.<br /><br /></li>
<li>Next, after we have everything downloaded and installed, we're going to create our first virtual machine, which is going to be our domain controller, which is going to house Active Directory.<br /><br /></li>
<li>We're going to give this virtual machine two network adapters: one is going to be used to connect to the outside internet, and the other one is going to be used to connect to the VirtualBox private network that the clients are going to connect to.<br /><br /></li>
<li>After our virtual machine is created, we're going to install Server 2019 on it and then we're going to assign IP addressing for the internal network. The external network will automatically get IP addressing from your home network or your home router, so we don't have to worry about properly subnetting and what/not (that&rsquo;s for a later video lol).<br /><br /></li>
<li>After we have IP addressing set up, we're going to name the server and then we're going to install Active Directory and create our domain.<br /><br /></li>
<li>Then, we're going to configure that and routing so the clients on the private network can reach the internet through the domain controller.<br /><br /></li>
<li>Next, we're going to set up DHCP on the domain controller so when we create our Windows 10 machine (which will automatically get an IP address).<br /><br /></li>
<li>The last thing we do on the domain controller before we create our client virtual machine is we're going to run a PowerShell script that will automatically create a thousand users in Active Directory, and I'll do a quick rundown through the script and explain what each line is so you can kind of get an intuition on how PowerShell is useful and what kind of things you can use it for.<br /><br /></li>
<li>After creating the users, we're going to create another virtual machine and install Windows 10 on it, and that virtual machine will be connected to the private VirtualBox network. We're going to name that machine "Client1" and join it to the domain, and then we're going to log into it with one of our domain accounts.</li>
</ol>
<br />
At this point, our tutorial is going to be pretty much concluded.
<br />
<br />
<br />
<br />




## Part 1: Installing VirtualBox

1.  Download VirtualBox from the website by opening the following link in a new tab: <https://www.virtualbox.org/>
    1.  Click blue Download button
    <p align="center">
    <br/>
    <img src="https://i.imgur.com/EsLsrn0.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br />



    2.  Choose the platform - **Windows hosts** for me.
    3.  When you're done downloading the installer, you're going to also want to download the **Extension Pack**:<br/>
    <p align="center">
    <br/>
    <img src="https://i.imgur.com/Jij6ysP.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br />
    <br/>
2.  While that is downloading: Checkout the SHA256 checksums links (within the green square on th screenshot) on this page to determine if the file has been tampered with in transit (during the download).
    1.  The SHA256 checksums link will open a new tab with a list of hashes (shown below).<br/>

<ul>
<li>Now the VirtualBox installer is in the Downloads folder:</li>
</ul>
<p align="center"><br /><img src="https://i.imgur.com/9ulDx0V.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p align="center">&nbsp;</p>
<p align="center">&nbsp;</p>
<ul>
<li>Open PowerShell, go to the downloads folder, type Get-FileHash followed by V then tab (file shows up). Hit enter to generate the SHA256 hash:</li>
</ul>
<p align="center"><br /><img src="https://i.imgur.com/y5oBk7b.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p align="center">&nbsp;</p>
<p align="center">&nbsp;</p>
<ul>
<li>Double-click and copy the hash from PowerShell and do a Ctrl-F and paste back in the new tab browser of SHA256 checksum hashes provided by Virtual Box:</li>
</ul>
<p align="center"><br /><img src="https://i.imgur.com/xjJNpbV.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /> </p>
<ul>
<li>As you can see, the hash from PowerShell matches the provided hash. So, we know for a fact that the file has not been tampered with in-transit.<br /><br /></li>
</ul>
    <br/>
    <br/>
    <br/>
3.  Now run the exe file from the download folder and follow the install wizard:<br/>
<br/>
<br/>
4.  Hit Next:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/pgvveld.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
5.  On the next page (Custom Setup) you can change the location of where VirtualBox is installed (I’m going to accept the defaults and hit Next):  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/yKj2Xh3.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
6.  The next screen will tell you that it will “reset you network connection and will temporarily disconnect you from the network”. Go ahead and hit Yes if you’re good with that:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/ikCcICC.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
7.  Hit Yes on the Python Core screen as well:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/ecg3HjB.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
8.  Select Install:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/v6amPxt.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
9.  Wait for it to complete:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/AKDykXX.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
10. When it’s done you’ll see the “Installation Complete” screen:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/lDtR4ZN.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
11. Finally hit Finish and VirtualBox will automatically pop on:  <br/>
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/ORalQJ2.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    <br/>
    <br/>



## Part 3: Configuring VirtualBox with the second Windows Server 2019 VM
<p>Let's now download <strong>Windows&nbsp;Server 2019 ISO</strong> by going to this link:&nbsp;<a href="https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019">https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019</a></p>
<ul>
<li>Make sure you choose the ISO:</li>
</ul>
<p>&nbsp;</p>
<p><img src="https://1drv.ms/i/s!AgcNOlrpw6Ukhb8degFONNe4Bcb7zQ?e=cynyLx" alt="" /><img src="https://i.imgur.com/27ptdtC.jpeg" alt="" width="80%" height="80%" /></p>
<ul>
<li>It will be the same process as the earlier Windows download.</li>
<li>Also, make sure to save it to the same place you saved your Windows 10 iso (just for ease).</li>
<li>When it's finished downloading, I'll meet back over at VirtualBox.</li>
</ul>
<p>&nbsp;</p>
<br/>
<br/>
<br/>

## Part 3.1: Create the Server 2019 VM
<p>Over to VirtualBox again, hit <strong>New</strong>:</p>
<p><img src="https://camo.githubusercontent.com/eea8dab18a4de44aed02aaa9c560ce7cb9c8dfa66c3f3f17f32be1e5228e6ce0/68747470733a2f2f692e696d6775722e636f6d2f6a715a367262352e706e67" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>Fill in the info shown below and then <strong>Next</strong>:</p>
<p><img src="https://i.imgur.com/9lAZDch.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>Name:&nbsp;<strong>DC-Server2019</strong></p>
<p>Folder:<strong> same as before</strong></p>
<p>ISO:<strong> the one just downloaded</strong></p>
<p><strong>Checkbox</strong> for Skip Unattended Installation</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /><br />For the &ldquo;Hardware&rdquo; screen I'll do the same and Hit <strong>Next&nbsp;</strong>(again keep in mind that this will be relying on your physical computer&rsquo;s resources):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/TQG3cTD.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /><br /> Just for the installation processe, I:</p>
<ul>
<li>Increased the Base Memory to <strong>4096 MB</strong> (4 Gigs)</li>
<li>Decreased the CPU to <strong>3 Processors </strong></li>
</ul>
<p style="text-align: left;"><em>(it just makes it go a whole lot quicker. I'll most likely reduce these number after installation. Again, If you notice these setting eating too much of you host's resources, by all means lower these)</em></p>
<p style="text-align: left;"><br /><br /><br /></p>
<p><br /><br /> For the Virtual Hard disk, I decreased to <strong>30 Gigs&nbsp;</strong>and&nbsp;Hit <strong>Next</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/VOtgko0.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p><br /><br /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Here's the summary page again:</p>
<p><strong><img style="display: block; margin-left: auto; margin-right: auto;" src="https://i.imgur.com/eR9X62G.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;Looks good.&nbsp;Hit <strong>Finish</strong></p>
<p style="text-align: center;">&nbsp;</p>
<p>&nbsp;</p>
<p>Then the new DC-Server2019 pops up in VirtualBox Manager:</p>
<p style="text-align: center;"><strong>&nbsp;<img src="https://i.imgur.com/zPkFLKM.png" alt="" width="80%" height="80%" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Before starting or domain controller first go to <strong>Settings</strong> &gt; <strong>Advanced</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/RL8GVRD.png" alt="" width="80%" height="80%" /></p>
<p>Here, change both <em>Shared Clipboard</em> and <em>Drag'n'Drop</em> to <strong>Bidirectional</strong>.</p>
<p><em>This simply means you can use Ctl C and&nbsp; Ctrl V (Copy and Paste) withing the VM.</em></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Lastly, before we start our domain controller Server 2019 vm, head over to <strong>Settings</strong> &gt; <strong>Network</strong></p>
<p>If you remember from the diagram, for our domain controller we&nbsp;want to have 2 network interface cards (NICs):</p>
<ol>
<li>One will be dedicated for the internet and will be running <em>Network Address Translation (NAT)</em>.</li>
<li>The other will be dedicated for the <em>internal VMware network.</em></li>
</ol>
<p align="center"><img src="https://i.imgur.com/x12bzqc.png" alt="Microsoft Azure Virtual Machine" width="90%" height="90%" /> </p>
<p><br /> </p>
<p>So the first adapter (Adapter 1) is the one that comes pre-configured. This will connect to the house's internet. Just leave that as <strong>NAT</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/QYAyPyi.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>We need to add one more adapter&nbsp;for the internal network. So make this change, go to:</p>
<p style="text-align: center;"><strong>Adapter 2 </strong>&gt;<strong> Enable Network Adapter </strong>&gt;<strong> Internal Network </strong>&gt;<strong> OK</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/JSh6bmd.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>So now our VM is pretty much configured, but it's still empty. We'll go ahead and <strong>double-click</strong> it to start it.</p>
<p>&nbsp;</p>
<p>You should see the VirtualBox screen (which means the VM is starting):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/wMFScbT.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>It will then start to load the iso. Just like before when we did the Windows 10 vm, this process takes awhile:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/nFaqu9P.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ft0vxCa.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>Finally, it is ready to configure. You will see the following screen:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/RhEsL2S.png" alt="" width="80%" height="80%" /></p>
<p>Hit Next</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now hit Install Now:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/LRlumJ4.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>Choose Windows Server 2019 Standard Evaluation (Desktop Experience):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/SL1Q8ux.png" alt="" width="80%" height="80%" /></p>
<p>If you choose the non-Desktop Experience options, you&rsquo;ll only get the command line).</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Agree to the terms:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/CEzxrjT.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>Now choose the second option&nbsp;<strong>Custom: Install Windows only (advanced)</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/TLtgSmq.png" alt="" width="80%" height="80%" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Hit <strong>Next</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/98kcx1T.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>It will start installing (this part will take a while):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/DZtWayg.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>The server will restart several times during this installation. Eventually it will get to a black screen that says, &ldquo;push any button to boot&rdquo;.</p>
<p>Don&rsquo;t press anything at this point and it will boot to windows on its own:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/w4xzdhV.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>When Server2019 in fully installed, you&rsquo;ll be prompted to put in an admin password. I just used <strong>Password1</strong> for this example and select Finish:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/cTXrdTP.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>The windows splash screen will show next. It is asking you to press Ctrl Alt Del. Because it&rsquo;s a virtual machine, you&rsquo;ll have to go to the VirtualBox Input tab on the top and select Ctrl Alt Del:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/HJUYvxa.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now we can login with our <strong>Password1</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/0iHMHxl.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Windows will apply a bunch of settings and will login:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/e7u6Dsd.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Hit <strong>Yes</strong> on the Network prompt on right:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ikrEBJ7.png" alt="" width="80%" height="80%" /></p>
Here is the HTML code with the integer values in the `width` and `height` attributes of the `img` tags replaced with `width="80%" height="80%"`:

<p>&nbsp;</p>
<p>The Server Manager will start up eventually. Go ahead and minimize it:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/gzBITmQ.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>You may notice that when we try to resize the VM, it stays small and not really user-friendly. This is where the Guest Edition Extension comes in which I had you download earlier. To start using the Guest Additions extension, go to <strong>Devices</strong> &gt;&gt; <strong>Insert Guest Additions CD image</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/B7ujwnF.png" alt="" width="80%" height="80%" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Then in the VM, go to <strong>File Explorer</strong> &gt;&gt; <strong>This PC</strong> &gt;&gt; <strong>CD Drive (D:) VirtualBox Guest Additions</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/oB4ZQqu.png" alt="" width="80%" height="80%" /></strong></p>
<p><strong>&nbsp;</strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Double-click on the file that ends with amd64:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/HRiRZiq.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>You&rsquo;ll be prompted with an Install Wizard in which you just want to accept all defaults and keep selecting <strong>Next</strong> until it starts installing:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Gdatqi9.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/deKK91j.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Select <strong>I want to manually reboot later </strong>&gt; <strong>Finish</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/6TgvVlP.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now Shutdown the VM:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/FIZPZ3P.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/6qCnS7S.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now Start DC-Server2019 again (either double-click on VM on left or use the green Start arrow):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/KOXci5V.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Insert Ctrl+Alt+Del again:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/G5eqab6.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Login with <strong>Password1</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/62iw3HR.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now we can resize the VM screen and the mouse doesn&rsquo;t lag like before:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/sjg0QBF.png" alt="" width="80%" height="80%" /></p>
<p>That&rsquo;s the benefit of installing the Guest Additions.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now we are going to setup our IP addressing. Looking back at the Network Diagram, we have two NICs. One that&rsquo;s dedicated to the internet and one that we&rsquo;re going to use for our internal network:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/pscHDRW.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>The one that's on the INTERNET will get an ip address automatically from your home router, so we don't have to do anything for that one. But for the Internal NIC, we will have to set up manually.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>So, on the VM, click on the Network icon on the <strong>bottom right in the system tray</strong> &gt;&gt; <strong>then click on Network</strong> when the popup shows:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Xkk9f0M.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Select <strong>Change adapter options</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/EvOYQqU.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now notice that we have two network adapters. So we have to figure out which one is which and name them appropriately because we&rsquo;ll be using them later when we&rsquo;re setting up routing:</p>
<p style="text-align: center;">&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/igymAbm.png" alt="" width="619" height="535" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>So, let&rsquo;s check out the Ethernet2 one (it might be named different for you). Right-click on <strong>Ethernet2</strong> &gt;&gt; <strong>Status</strong> &gt;&gt; <strong>Details</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/QjPtBYI.png" alt="" width="556" height="461" /></p>
<p>This looks like your proper home ip address, the one that&rsquo;s connected to your home network.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>So, let&rsquo;s rename this one by: <strong>right-clicking</strong> &gt;&gt; <strong>rename</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/3ohgbIt.png" alt="" width="559" height="217" /></p>
<p>Then rename it to <strong>_INTERNET_</strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now let&rsquo;s look at the other adapter Ethernet:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Sc7sZ6z.png" alt="" width="556" height="461" /></p>
<p>This adapter is the internet adapter and has been given an Autoconfiguration IPv4 address of 169.254.164.48. This basically means that this adapter was looking for a DHCP server in order to get an IP address, but it was unable to find the DHCP server. So, we know it&rsquo;s the internal adapter.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>So go ahead and rename this adapter to <strong>x_INTERNAL_x</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/szvzGg1.png" alt="" width="559" height="459" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now we will give an ip address to our <strong>x_INTERNAL_x</strong> adapter.</p>
<p>Right-click on the <strong>internal adapter</strong> &gt;&gt; <strong>Properties</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/XJdzknt.png" alt="" width="776" height="642" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Double-click on <strong>Internet Protocol Version 4</strong> &gt;&gt; <strong>Use the following IP address</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/62U26Xa.png" alt="" width="765" height="548" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Input the ip info from the Network Diagram:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/q9A5M1v.png" alt="" width="973" height="603" /></p>
<p>&nbsp;</p>
<p>We're going to assign the following ip address to our Internal:</p>
<p>172.16.1 and then the mask 255.255.0.0</p>
<p>We're not going to use a default gateway because the domain controller itself is going to serve as the default gateway. Remember the domain controller has two NICs, one on the internet and one on the inside network.</p>
<p>For DNS server, when we install Active Directory, it automatically installs DNS, so for right now, configure this internal adapter to use itself as the DNS server. So, you can either enter its own ip address but even better would be to enter its loopback address of 127.0.0.1 which is kind of a generic address that refers to itself and it always up. So, whenever a computer pings like 127.0.0.1, they're actually pinging themselves.</p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/sXaBFR1.png" alt="" width="513" height="366" /></p>
<p style="text-align: left;">Hit <strong>OK</strong></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>Now let&rsquo;s rename this PC:</p>
<p>right click the <strong>Start menu</strong> &gt;&gt; <strong>System</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/EAodX3O.png" alt="" width="339" height="458" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Click <strong>Rename this PC</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/0JylR43.png" alt="" width="603" height="541" /></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>Change the name to <strong>DC</strong> (which stands for Domain Controller):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/0R3xwu9.png" alt="" width="426" height="188" /></p>
<p>Hit <strong>Next</strong>:</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Hit <strong>Restart Now</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/OsZgQCX.png" alt="" width="425" height="131" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>The VM will restart:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/x2qfRoP.png" alt="" width="633" height="520" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Insert <strong>Ctrl+Alt+Del</strong> again:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/gBSv0bD.png" alt="" width="604" height="491" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Login with <strong>Password1</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/SNsSoIN.png" alt="" width="603" height="491" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>Install Active Directory Domain Services and Create a Domain:&shy;&shy;</h2> <p>Now we are going to install Active Directory Domain Services&shy;&shy; and then we&rsquo;re going to create a domain:</p> <p>&nbsp;</p> <p style="text-align: center;"><img src="https://i.imgur.com/WXkOQ1z.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Click on <strong>Add roles and features</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/q6iUDM8.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong> on the first screen:</p> <p style="text-align: center;"><img src="https://i.imgur.com/HbkiMQQ.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/JYj6Zu9.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>And this is where you pick the server where you want to install Active Directory. Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/M14kShK.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Choose <strong>Active Directory Domain Services</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/m6HzPPR.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Then <strong>Add Features</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/d9KUn0Y.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/dn4zwir.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/KjUVddx.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit&nbsp;<strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/DKNhuVx.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Then hit <strong>Install</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/rspLnQO.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Depending on how much RAM and CPUs you gave your VM, this could take a while, but you&rsquo;ll get there:</p> <p style="text-align: center;"><img src="https://i.imgur.com/OhV4nMg.png" alt="" width="80%" height="80%" /></p> <p style="text-align: left;">&nbsp;</p> <p style="text-align: left;">60 seconds later:</p> <p style="text-align: center;"><img src="https://i.imgur.com/wfJd5AG.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Another 30 seconds...When the Role is finished installing, hit <strong>Close</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/ivDzlew.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p>

<p>You&rsquo;ll now see a little <strong>yellow flag</strong> in top right corner, click the <strong>yellow flag</strong> and then click on <strong>Promote this server to a domain controller</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/eDvwpri.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>On the next popup select <strong>Add a new forest </strong>and give the Root domain name <strong>mydomain.com </strong>and hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/Z4rakBL.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>On the next screen just put <strong>Password1</strong> as the password (we&rsquo;re probably never going to use this) and hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/HxN9UxC.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/27Ig5eE.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>On the next screen you&rsquo;ll notice the NetBIOS name auto populates to MYDOMAIN, go ahead and hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/j1OL3OR.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/3isUgZK.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Hit <strong>Next</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/clehhui.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>It will run some checks and then you can hit <strong>Install</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/eKilfc8.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Install starts:</p> <p style="text-align: center;"><img src="https://i.imgur.com/g9fGyQm.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>When it&rsquo;s finished, it will automatically restart:</p> <p style="text-align: center;"><img src="https://i.imgur.com/PajLckQ.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p style="text-align: center;"><img src="https://i.imgur.com/mJ5pQfo.png" alt="" width="80%" height="80%" /></p> <p style="text-align: left;">&nbsp;</p> <p style="text-align: left;">&nbsp;</p> <p style="text-align: left;">&nbsp;</p> <p>Restarting:</p> <p style="text-align: center;"><img src="https://i.imgur.com/JCpa2qG.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>After forever :) we can finally login again. Issue the <strong>Ctrl+Alt+Del</strong> again:</p> <p style="text-align: center;"><img src="https://i.imgur.com/8cTmv0V.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Notice we now have MYDOMAIN\Administrator which we didn&rsquo;t have before. Login with <strong>Password1</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/RkXDikM.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Now we're going to create our own dedicated domain admin account instead of using the built-in administrator account. We can do that by going to <strong>Start</strong> &gt; <strong>Windows </strong><strong>Administrative Tools</strong> &gt; <strong>Active Directory Users and Computers</strong>:</p> <p style="text-align: center;"><img src="https://i.imgur.com/b5F3iH4.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>Notice our brand new mydomain.com:</p> <p style="text-align: center;"><img src="https://i.imgur.com/gcAIeiQ.png" alt="" width="80%" height="80%" /></p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p> <p>&nbsp;</p>


<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Notice our brand new mydomain.com:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Zjx9ekI.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Let's create an Organization Unit (OU) by <strong>right-clicking on mydomain.com</strong> &gt;&gt; <strong>New</strong> &gt;&gt;&nbsp;<strong>Organization Unit</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/WjQaC9G.png" alt="" width="80%" height="80%" /></p>
<p>Think of an&nbsp;OU as a folder within Active Directory.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Name the OU <strong>_ADMINS</strong></p>
<p>Then uncheck<strong> <strong>Protect container from accidental deletion</strong> </strong>&gt;&gt;<strong> <strong>OK</strong></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p><img src="https://i.imgur.com/OirUxaY.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Next create a new user within&nbsp;_ADMINS:</p>
<p><strong>right-click _ADMINS</strong> &gt;&gt; <strong>New</strong> &gt;&gt; <strong>User</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/ndYog6I.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">I'll use my name for this example.</p>
<p style="text-align: left;">Fill in the First and Last Name:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/POe39VZ.png" alt="" width="80%" height="80%" /></p>
<p>Notice the <strong>a-</strong>&nbsp;I inserted in the User logon name field. That's a common practice to denote an administrator account.</p>
<p>Hit <strong>Next</strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Use <strong>Password1</strong> &gt;&gt; uncheck <strong>User must change password at next logon</strong> &gt;&gt; check <strong>Password never expires</strong> &gt;&gt;&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/nNNFX1O.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Click <strong>Finish</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/RpprAy6.png" alt="" width="80%" height="80%" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Notice our new user account shows up:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/1WfduGr.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>The new user account is not an admin yet, so to do so:</p>
<p><strong>right-click account name</strong> &gt;&gt; <strong>Properties</strong> &gt;&gt; <strong>Member Of</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/gsKtiPo.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>From here click on <strong>Add&nbsp;</strong>&gt;&gt; type in <strong>domain&nbsp;admins</strong> &gt;&gt; <strong>Check Names&nbsp;</strong>&gt;&gt; <strong>OK</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/Lj6tFfG.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Hit <strong>Apply&nbsp;</strong>&gt;&gt;&nbsp;<strong>OK</strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/Fbt5xMq.png" alt="" width="80%" height="80%" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Now we have our very own Domain Admin account:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ZpJM2wT.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>To use this new Domain Admin account, Sign Out of the current Windows user that your signed into:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/DI7mh9z.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/tuve4c8.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>It logs you out and brings you to the logon screen:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/AYEup1k.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>Input</strong> &gt;&gt; <strong>Ctrl+Alt+Del</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/0TLnbyl.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Instead of logging into the current Administrator account, select <strong>Other user</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/NaazIC8.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Use our new domain admin account:</p>
<p>Username: <strong>a-aguild</strong></p>
<p>Password:&nbsp;<strong>Password1</strong></p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/baRZ5kq.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>The new domain admin account credentials is pulled from Active Directory and let's us login:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/svPfWb4.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>STEP 7:</h2>
<p>Now, we're going to configure the Domain Controller for routing so the clients on the private network can reach the internet through the domain controller.</p>
<p>Looking back at our network map, the next thing we're going to do is install <strong>RAS/NAT (Remote Access Server/Network Address Translation)</strong>.</p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/UkkJ58e.jpeg" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>The purpose of this installation is to enable our Windows 10 client to be on a private virtual network while still having access to the internet through the domain controller.</p>
<p>By installing RAS and NAT on the domain controller, we will allow our clients to achieve this functionality.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>To do this, go to <strong>Add roles and features</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/KpZXEU5.png" alt="" width="80%" height="80%" /></strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Hit <strong>Next</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/Steucxs.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Hit&nbsp;<strong><strong>Next</strong></strong></p>
<p style="text-align: center;"><strong><strong><img src="https://i.imgur.com/xCqIxtR.png" alt="" width="80%" height="80%" /></strong></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">This is our server, hit<strong> Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/9fInicl.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">For Roles select&nbsp;<strong>Remote Access&nbsp;</strong>&gt;&gt;&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/IyPGrLd.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit <strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/BTl45Sl.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit <strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/vVdXqWM.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Install&nbsp;<strong>Routing&nbsp;</strong>&gt;&gt;&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/9Z0sEch.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit&nbsp;<strong>Add Features</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/u0PoU6G.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Select<strong> Install&nbsp;</strong>on the Confirmation page (not shown here)</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">Installation starts (this could take a long minute, so go grab some coffee and meet me back here in 5):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/HnD8Lyf.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>Finally it's done installing &gt;&gt; hit&nbsp;<strong>Close</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/2oHFusy.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Go to <strong>Tools</strong> &gt;&gt; <strong>Routing and Remote Access</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/jjgT3Kh.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>The following screen opens:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/e1k1ON7.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;"><strong>Right-click on DC (local)</strong> &gt;&gt; <strong>Configure and Enable Routing and Remote Access</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/7HfpiCd.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/L5OfDyz.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Select <strong>Network address translation (NAT)</strong> &gt;&gt;&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/mvtHX1V.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Now when you get to this screen, it suppossed to show both of our internet adapters. For some reason it's greyed out (it hasn't synced yet).</p>
<p style="text-align: left;">So you'll have to hit&nbsp;<strong>Cancel&nbsp;</strong>and redo the last couple steps (happens everytime LOL. It's just some wonky bug)</p>
<p style="text-align: center;"><img src="https://i.imgur.com/oIjqoZK.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">So I just Canceled and did the last two steps again and the adapters showed up this time:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/3hr710u.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Choose the <strong>_INTERNET_</strong> interface and hit <strong>Next</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/DZOv1XX.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Then select&nbsp;<strong>Finish</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/HY7KjeQ.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">It will take a second:</p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/Uin0KfH.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Now we'll see a little green star icon on the DC (local) which means it's up and running correctly:</p>
<p><img src="https://i.imgur.com/4BHeyvH.png" alt="" width="80%" height="80%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>If we look at our network diagram again,&nbsp;we have our <strong>Domain / AD DS</strong> setup and then we just configured <strong>RAS and NAT&nbsp;</strong><br />so Step 7 is finished now (next is DHCP setup):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/UkkJ58e.jpg" alt="" width="80%" height="80%" /></p>

<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>STEP 8: DHCP Scope Configuration on DC</h2>
<p>Next, we're going to set up a DHCP Server on the domain controller and&nbsp;what this is going to do is allow our windows 10 clients to get an ip address that will let them&nbsp;get on the internet and browse the internet (even though they're on this private internal network just like in your office or school.</p>
<p>&nbsp;</p>
<p>So to setup our DHCP Server we'll go back to the <strong>domain controller</strong> &gt;&gt; <strong>Add roles and features</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/iHsi3Dn.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit <strong>Next</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/MybTiJs.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p>Hit&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/jLZHkdB.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">This is our server.&nbsp;Hit&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/DSCtYzi.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Check the box next to&nbsp;<strong>DHCP Server</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/mpA7KPf.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit&nbsp;<strong>Add Features</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/PK6DcqB.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>Hit&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/UcrdR1W.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;"><strong>Hit&nbsp;<strong>Next 3 times </strong></strong>until you get to this screen...then select<strong><strong> Install</strong></strong></p>
<p style="text-align: center;"><strong><strong><img src="https://i.imgur.com/38MYsWq.png" alt="" width="80%" height="80%" /></strong></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;"><strong><strong>Installation in progress</strong></strong></p>
<p style="text-align: center;"><strong><strong><img src="https://i.imgur.com/7YFoCwk.png" alt="" width="80%" height="80%" /></strong></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">When DHCP is finished installing hit<strong> Close</strong></p>
<p style="text-align: center;"><strong><img src="https://i.imgur.com/ZpxPNMp.png" alt="" width="80%" height="80%" /></strong></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Go to&nbsp;<strong>Tools&nbsp;</strong>&gt;&gt;&nbsp;<strong>DHCP</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/IW0iE2e.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">Here's our dhcp control panel:</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/mOwnU7j.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">So the whole purpose of dhcp is to allow the computers on the network (like client computers) to automatically get their ip addresses.</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">So looking at our diagram here we defined the scope:</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/UkkJ58e.jpg" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">So next we're going to create a scope that will give the ip addresses in the range of 172.16.0.100 to&nbsp;172.16.0.200 with a&nbsp;subnet mask of 255.255.255.0</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">If you hit the dropdown arrow on dc.mydomain.com, you'll see that they have a red icon next to IPv4 and IPv6 (meaning that they are down):</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/SuaiZft.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0"><strong>Right-click on IPv4</strong> and select <strong>New Scope</strong>:</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/WE3qe3n.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">Hit&nbsp;<strong>Next&nbsp;</strong>on first Wizard screen (not shown here)</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">Name the scope&nbsp;<strong>172.16.0.100-200&nbsp;</strong>and hit&nbsp;<strong>Next</strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/KHvAF4i.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">Fill in the <strong>Start and End IP Address</strong>&nbsp;with our scope, as well as change the Subnet Mask to the CIDR value of <strong>24</strong> (which is the same as 255.255.255.0 in Dotted Decimal Notation) and hit&nbsp;<strong>Next</strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/sbQpLy4.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">I can't really go into the fundamentals of IP addressing in this tutorial, but if you're interested in learning the quick way, you can check out my other tutorial <strong>IP Addressing: The Art of Subnetting</strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">The next screen gives us the option to exclude IP addressess from our range, but we don't really need to worry about that, so hit&nbsp;<strong>Next</strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><strong><img src="https://i.imgur.com/iSWyTHp.png" alt="" width="80%" height="80%" /></strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0"><strong>Lease Duration</strong> is how long a computer can have that ip address before it needs to be refreshed. This just depends on your use case.</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0"><strong>For example</strong>:</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">If you're running a cafe like a starbucks and you have a lease time of 8 Days, say somebody comes and gets on the wi-fi network, drinks a coffee and then leaves after 30 minutes. This means that the IP address that was given to that quick coffee drinker of 8 days is going be tied up until the 8&nbsp; days runs out and no one else can have that ip address until the lease expires. So if you're like running a Starbucks you may want to make the lease like 2 hours instead of 8 days. But if you're just on a home lab like we're doing here, 8 days is fine so we're just going to hit&nbsp;<strong>Next</strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/X559P0N.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">Hit&nbsp;<strong>Next&nbsp;</strong>to <strong>Yes. I want to&nbsp;Configure DHCP Options now</strong>:</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/23QBF5T.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">For the Router (Default Gateway) enter&nbsp;<strong> 172.16.0.1&nbsp;</strong>&gt;&gt;&nbsp;<strong>Add&nbsp;</strong>&gt;&gt;&nbsp;<strong>Next</strong></div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/HjLw3wi.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">When you select&nbsp;<strong>Add</strong> it drops the IP address down to the bottom text area (<strong>Don't Forget to Do This!</strong>):</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0"><img src="https://i.imgur.com/o6X0JMH.png" alt="" width="80%" height="80%" /></div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: center;" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
</div>
<div class="segment style-scope ytd-transcript-segment-renderer" style="text-align: left;" tabindex="0">&nbsp;</div>


<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Then add&nbsp;<strong>172.16.0.1&nbsp;</strong>to the IP address list and hit&nbsp;<strong>Next</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/nZJQUrI.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit&nbsp;<strong>Next</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/IEtTZXj.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Hit&nbsp;<strong>Next&nbsp;</strong>to confirm you want to activate this scope now:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ia3E4Yf.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Finally hit&nbsp;<strong>Finish</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/0EwdUR9.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Notice the message in the middle box (it's telling us we must authorize the DHCP Server in Active Directory):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/QnFegf0.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">To authorize, <strong>right-click dc.mydomain.com</strong> and select <strong>Authorize</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/8G2BAlp.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Then&nbsp;<strong>right-click IPv4&nbsp;</strong>&gt;&gt;&nbsp;<strong>Refresh</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/a9zsdZZ.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Now notice our IPv4 and IPv6 has a green icon next to it:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/mFTpkMZ.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">If you click on the&nbsp;<strong>Scope&nbsp;</strong>&gt;&gt;&nbsp;<strong>Address Leases</strong>. You'll see there are no leased IP addresses yet. But when we create our Windows 10 client computer VM, you'll see it lease an IP address from here:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/3hM62c3.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<h2 style="text-align: left;">STEP 9: Run a PowerShell script that will automatically create a thousand users in Active Directory</h2>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">So before we actually go and create our create our client computer and join it to the domain, we're first we're going to use a powershell script to create a whole bunch of users in active directory so we can have a bunch of sample users and we don't have to manually create a whole</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">bunch of them.</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">To do that</div>

<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>To do that, refer to the AD_PS-master.zip file that's in this repository:</p>
<p>&nbsp;</p>
<p style="text-align: center;"><img src="https://i.imgur.com/S4MBu6b.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Click on that zip file:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/2pVblUy.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Then&nbsp;<strong>Download raw file</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/6nsuXdk.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Open the zip file:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/lyCRz9D.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">Then click on Extract all and save it wherever you like:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/dUmPqzC.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">Open the extracted folder file:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Fzw72Ww.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">And you'll see the files with&nbsp;the PowerShell scripts and a text file. You'll see there's a plain text file called&nbsp;<strong>names.txt</strong>&nbsp;&gt;&gt; <strong>let's open this first</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Ui0eZOA.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">This <strong>names.txt</strong> file basically has a thousand randomized names in it from a name generator I used from a college classmate:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ddiprsk.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">&nbsp;</div>
<div class="segment style-scope ytd-transcript-segment-renderer" tabindex="0">For Ss and Gs, at the very top go ahead and add your own name like I did. We're going to use this file to programmatically create all of these users and we're going to create one for ourselves too. So go ahead and put your name at the top or whatever name comes to mind, then <strong>save the file</strong>:</div>
<p style="text-align: center;"><img src="https://i.imgur.com/ghfjcdQ.png" alt="" width="80%" height="80%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>



## STEP 10: Configuring VirtualBox with our Windows 10 VM Client Computer
<ul>

<ol>
1. Create a Windows 10 image file for the VirtualBox OS (you can do this many different ways):<br/>
    <br/>
<ol style="list-style-type: lower-alpha;">
<li>Go to the Microsoft download center by opening the following link in a new tab:<a href="https://www.microsoft.com/en-ca/software-download/windows10">https://www.microsoft.com/en-ca/software-download/windows10</a><br /><br /></li>
<li>It might prompt you to Login or fill out some info like your name, email, job title, etc...(but it's totally legit)<br /><br /><br /></li>
<li>Scroll down and select the &ldquo;Download tool now&rdquo; button:<br /><br /><br /></li>
           <p align="center">
    <br/>
    <img src="https://i.imgur.com/asglkSp.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

    
<li>Open the download:<br /><br /><br /></li>
   <p align="center">
    <br/>
    <img src="https://i.imgur.com/2N7KlH5.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
<li>Eventually you&rsquo;ll see this screen in which you need to Accept:<br /><br /><br /></li>
       <p align="center">
    <br/>
    <img src="https://i.imgur.com/zU43CPw.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
<li>On the &ldquo;What do you want to do?&rdquo; screen, select the second option and hit Next:<br /><br /><br /></li>
       <p align="center">
    <br/>
    <img src="https://i.imgur.com/HAsyCZC.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
<li>The next screen you&rsquo;ll be able to change language, architecture and edition if you need to (I&rsquo;m just going to hit next):<br /><br /><br /></li>
       <p align="center">
    <br/>
    <img src="https://i.imgur.com/qFPYiql.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
<li>The next screen lets you choose between a USB flash drive or an ISO file. Select ISO file, hit Next and save it wherever you like:<br /><br /><br /></li>
       <p align="center">
    <br/>
    <img src="https://i.imgur.com/k4avGVy.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
<li>Once you&rsquo;ve chosen a download folder it will begin:<br /><br />Note: When the installation is complete, you have to hit Finish on the screen that mentions &ldquo;burning to a DVD&rdquo; (at least I had to). All that does is create a drive in File Explorer called DVD Drive.</li>
       <p align="center">
    <br/>
    <img src="https://i.imgur.com/Xo3GfmN.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>
    
</ol>
</li>
</ol>
</li>
</ul>
<br/>
<br/>


## STEP 10.1: Create the Windows 10 VM Client Computer
<p>&nbsp;</p>
<p>While that&rsquo;s downloading, hop over to VirtualBox and we&rsquo;ll start creating the VM:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/jqZ6rb5.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p><br /><br /> Hit the New star-looking button at the top:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/a0aCej0.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p><br /><br /> Give your VM a Name and specify where you want to store VM files:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/v2MjLCd.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<ul>
<li>Name: <strong>Windows10-VM-mydomain</strong></li>
<li>Folder: <strong>C:\Users\guild\VirtualBox VMs</strong></li>
<li>ISO Image: <strong>C:\Users\guild\VirtualBox VMs\Windows.iso</strong> (choose location where you saved it to)</li>
<li>Skip Unattended Installation: <strong>Check the box</strong> (this allows to install the OS manually)</li>
</ul>
<p>&nbsp;</p>
<p><br /><br /><br /> The next &ldquo;Hardware&rdquo; screen allows you to configure the VM&rsquo;s memory and CPU Processors. (keep in mind that this will be relying on your physical computer&rsquo;s resources):</p>
<p style="text-align: center;"><img src="https://i.imgur.com/TQG3cTD.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /><br /> Just for the installation processe, I:</p>
<ul>
<li>Increased the Base Memory to <strong>4096 MB</strong> (4 Gigs)</li>
<li>Decreased the CPU to <strong>3 Processors </strong></li>
</ul>
<p style="text-align: left;"><em>(it just makes it go a whole lot quicker....trust me. If you notice these setting eating too much of you host's resources, by all means lower these)</em></p>
<p style="text-align: left;"><br /> Hit <strong>Next</strong> <br /><br /></p>
<p><br /><br /> For the Virtual Hard disk, I decreased to <strong>30 Gigs</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/VOtgko0.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p><br /> Hit <strong>Next</strong> </p>
<p>&nbsp;</p>
<p><br /><br /> Finally you&rsquo;ll see a Summary of what you just configured:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/xLEvIvO.png" alt="" width="718" height="382" /></p>
<p><br /> If you&rsquo;re good to go, click <strong>Finish</strong> <br /><br /><br /></p>
<p>&nbsp;</p>
<p>Shortly you should see the new Windows10 virtual machine in VirtualBox:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/AfUDgv1.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p><br />Go ahead and hit the green arrow labeled <strong>Start</strong> at the top. <br /><br /></p>
<p>&nbsp;</p>
<p><br /> Eventually, you&rsquo;ll be presented with the Windows Setup page:</p>
<p style="text-align: center;"><br /><img src="https://i.imgur.com/piVokV5.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>Hit <strong>Next</strong> <br /><br /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /> Hit <strong>Install Now</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/C0Cu9lA.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Once you&rsquo;re presented with Activate Windows screen; select &ldquo;<strong>I don&rsquo;t have a product key</strong>&rdquo; at the bottom to the left of the Next button:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/139DaiG.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> For the operating system option, select <strong>Windows 10 Pro</strong>:</p>
<p style="text-align: center;"><br /><img src="https://i.imgur.com/j1kgeQT.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>Hit <strong>Next</strong> </p>
<p>&nbsp;</p>
<p><br /><br /> <strong>Accept the license terms</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/atn5HDr.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p><br />Hit <strong>Next</strong> </p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Now choose the second option <strong>Custom: Install Windows only (advanced)</strong></p>
<p style="text-align: center;"><img src="https://i.imgur.com/tQdQFKe.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Hit <strong>next</strong> on the following screen:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/EkBjFc3.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Now you will see the installation of Windows 10 has started:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/y5YPtpz.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Windows will restart automatically to finalize installation:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/RqOYd65.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Choose your region (<strong>United States</strong>):</p>
<p style="text-align: center;"><br /><img src="https://i.imgur.com/MZFxlUP.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p>Hit&nbsp;<strong>Yes</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/K6GBSdD.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /><br /><br /></p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">&nbsp;</p>
<p style="text-align: left;">Hit <strong>Skip</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/16vfa5Q.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Select &ldquo;<strong>Set up for personal use</strong>&rdquo; and &ldquo;<strong>Next</strong>&rdquo;:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/zSzJQXm.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /> Select &ldquo;<strong>Offline account</strong>&rdquo; on bottom left:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/AR4rmuk.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Then &ldquo;<strong>Limited experience</strong>&rdquo;:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/WbUbVn3.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Input a username and hit Next:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ZfFlF1N.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Input a Password and hit Next:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/ThUV5vG.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Confirm your Password:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/dLNYvbI.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Answer the Security Questions:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/Y2HQnmb.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Select &ldquo;<strong>Not now</strong>&rdquo;:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/yFb9aJ1.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> You can do what you like here, but I always toggle these to No:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/8ssdysG.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p><br /><br /> Hit <strong>Skip</strong>:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/o96OZDB.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Select &ldquo;<strong>Not now</strong>&rdquo;:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/MarvDl1.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><br /><br /> Finally, we&rsquo;re up and running:</p>
<p style="text-align: center;"><img src="https://i.imgur.com/thcIT4l.png" alt="Microsoft Azure Virtual Machine" width="70%" height="70%" /></p>
<p><br /><br /> Create a username and password once Windows is done installing (below is my example)</p>
<ul>
<li>Username: aaron</li>
<li>Password: Neverhappened86</li>
</ul>
<br/>
<br/>
<br/>
<br/>

