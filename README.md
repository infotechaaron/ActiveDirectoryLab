# Active Directory Deployment and Bulk User Creation using PowerShell

**YouTube Demonstration – coming soon!**

## Description

In this lab we're to walk through how to create an Active Directory home lab Environment using Oracle Virtual Box. Configuring and running this lab will definitely help develop your understanding of how active directory and windows networking works, so I'd highly recommend running through it a couple times and then eventually try to build it on your without using this tutorial.

## Languages and Programs Used

-   **PowerShell**
-   **Oracle VirtualBox**

## Environments Used

-   **Windows 10 ISO**
-   **Windows Server 2019 ISO**

## Network Diagram:
<p align="center">
<br/>
<img src="https://i.imgur.com/x12bzqc.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
<br /></p>
<br />

#### **Part 1: Installing VirtualBox**

1.  Download VirtualBox from the website by opening the following link in a new tab: <https://www.virtualbox.org/>
    1.  Click blue Download button
    2.  Choose the platform (Windows hosts for me)<br/>
    <br/>
2.  While that is downloading: Checkout the SHA256 checksums link on this page to determine if the file has been tampered with in transit (during the download):  
    <p align="center">
    <br/>
    <img src="https://i.imgur.com/yqIChyh.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br />

The SHA256 checksums link will open a new tab with a list of hashes (shown below).<br/>

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

#### **Part 2: Configuring VirtualBox with a Windows OS**
<ul>

<ol>
1. Create a Windows 10 image file for the VirtualBox OS (you can do this many different ways):<br/>
    <br/>
<ol style="list-style-type: lower-alpha;">
<li>Go to the Microsoft download center by opening the following link in a new tab:<a href="https://www.microsoft.com/en-ca/software-download/windows10">https://www.microsoft.com/en-ca/software-download/windows10</a><br /><br /></li>
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


### Create Windows VM

2.  While that’s downloading, hop over to VirtualBox and we’ll start creating the VM:
          <p align="center">
    <br/>
    <img src="https://i.imgur.com/jqZ6rb5.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

    1.  Hit the New star-looking button at the top:  
          <p align="center">
    <br/>
    <img src="https://i.imgur.com/a0aCej0.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    2.  Give your VM a Name and specify where you want to store VM files:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/v2MjLCd.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
        <p>
        Name: **Windows10-VM-mydomain**  
        Folder: **C:\Users\guild\VirtualBox VMs**
        ISO Image: **C:\Users\guild\VirtualBox VMs\Windows.iso** (choose location where you saved it to)   
        Skip Unattended Installation: **Check the box** (this allows to install the OS manually)
        </p>

    3.  The next “Hardware” screen allows you to configure the VM’s memory and CPU Processors (keep in mind that this will be relying on your physical computer’s resources):  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/TQG3cTD.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
        Just for the installation processed, I increased the Base Memory to 4096 MB (4 Gigs) and increased the CPU to 3 Processors (it just makes it go a whole lot quicker....trust me. If you notice these setting eating too much of you host's resources, by all means lower these.)

        </p>Hit Next</p>

    5.  For the Virtual Hard disk, I decreased to 30 Gigs:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/VOtgko0.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

  
          
        <p>Hit Next</p>

    6.  Finally you’ll see a Summary of what you just configured:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/AfUDgv1.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/> 
        <p>If you’re good to go, click Finish</p>


    7.  Shortly you should see the new Windows10 virtual machine in VirtualBox:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/jqZ6rb5.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

  
          
        <p>Go ahead and hit the green arrow labeled Start at the top.</p>



    8.  Eventually, you’ll be presented with the Windows Setup page:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/piVokV5.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

  
        <p>Hit Next</p>


    9.  Hit Install Now:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/C0Cu9lA.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    10.  Once you’re presented with Activate Windows screen; select “I don’t have a product key” at the bottom to the left of the Next button:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/139DaiG.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    11. For the operating system option, select Windows 10 Pro:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/j1kgeQT.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

  
        <p>Hit Next</p>


    12. Accept the license terms:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/atn5HDr.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>

  
        <p>Hit Next</p>


    13. Now choose the second option Custom: Install Windows only (advanced)  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/tQdQFKe.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    14. Hit next on the following screen:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/EkBjFc3.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    15. Now you will see the installation of Windows 10 has started:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/y5YPtpz.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    16. Windows will restart automatically to finalize installation  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/RqOYd65.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    17. Choose your region (United States):  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/MZFxlUP.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    18. Hit Yes:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/K6GBSdD.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    19. Hit Skip:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/16vfa5Q.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    20. Select “Set up for personal use” and “Next”:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/zSzJQXm.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    21. Select “Offline account” on bottom left:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/AR4rmuk.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    22. Then “Limited experience”:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/WbUbVn3.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    23. Input a username and hit Next:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/ZfFlF1N.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    24. Input a Password and hit Next:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/ThUV5vG.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    25. Confirm your Password:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/dLNYvbI.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    26. Answer the Security Questions:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/Y2HQnmb.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    27. Select “Not now”:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/yFb9aJ1.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    28. You can do what you like here, but I always toggle these to No:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/8ssdysG.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    29. Hit Skip:  
          <p align="center">
    <br/>
    <img src="https://i.imgur.com/o96OZDB.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    30. Select “Not now”:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/MarvDl1.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>


    31. Finally, we’re up and running:  
                  <p align="center">
    <br/>
    <img src="https://i.imgur.com/thcIT4l.png" height="70%" width="70%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br/>
    <br/>



Create a username and password once Windows is done installing:  
  
Username: aaron  
Password: Neverhappened86

