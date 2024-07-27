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
<img src="https://i.imgur.com/x12bzqc.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
<br /></p>
<br />


Just to give a high-level overview of what we're going to do in this lab, I’ll have you first refer to the network diagram so that you can look at to get an idea of what’s accomplished in this tutorial.

<ul>
<li>The first thing we're going to do is download and install Oracle VirtualBox, which is what we're going to use to run our virtual machines.</li>
<li>After that's installed, we're going to download a Windows 10 ISO and a Server 2019 ISO that we're going to use to install the two operating systems on two separate virtual machines.</li>
<li>Next, after we have everything downloaded and installed, we're going to create our first virtual machine, which is going to be our domain controller, which is going to house Active Directory.</li>
<li>We're going to give this virtual machine two network adapters: one is going to be used to connect to the outside internet, and the other one is going to be used to connect to the VirtualBox private network that the clients are going to connect to.</li>
<li>After our virtual machine is created, we're going to install Server 2019 on it and then we're going to assign IP addressing for the internal network. The external network will automatically get IP addressing from your home network or your home router, so we don't have to worry about properly subnetting and what/not (that&rsquo;s for a later video lol).</li>
<li>After we have IP addressing set up, we're going to name the server and then we're going to install Active Directory and create our domain.</li>
<li>Then, we're going to configure that and routing so the clients on the private network can reach the internet through the domain controller.</li>
<li>Next, we're going to set up DHCP on the domain controller so when we create our Windows 10 machine (which will automatically get an IP address).</li>
<li>The last thing we do on the domain controller before we create our client virtual machine is we're going to run a PowerShell script that will automatically create a thousand users in Active Directory, and I'll do a quick rundown through the script and explain what each line is so you can kind of get an intuition on how PowerShell is useful and what kind of things you can use it for.</li>
<li>After creating the users, we're going to create another virtual machine and install Windows 10 on it, and that virtual machine will be connected to the private VirtualBox network. We're going to name that machine "Client1" and join it to the domain, and then we're going to log into it with one of our domain accounts.</li>
</ul>

At this point, our tutorial is going to be pretty much concluded.
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

## Part 2: Configuring VirtualBox with both a Windows 10 VM and a Windows Server 2019 VM
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


## Create Windows VM
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
