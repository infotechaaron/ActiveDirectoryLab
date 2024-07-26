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

1.  While that’s downloading, hop over to VirtualBox and we’ll start creating the VM:  
    ![A screenshot of a computer Description automatically generated](media/dc204c380fb38e0d8526410789d5ef75.png)
    1.  Hit the New star-looking button at the top:  
        ![A screenshot of a computer Description automatically generated](media/3f0831a144afddadf8a9cfb22f848b20.png)
    2.  Give your VM a Name and specify where you want to store VM files:  
        ![](media/01bdfefc0b18e6835f69bb5603bbe39b.png)  
        Name: **Windows10-VM-mydomain**  
        Folder: C:\\Users\\guild\\VirtualBox VMs   
        ISO Image: C:\\Users\\guild\\VirtualBox VMs\\Windows.iso (choose location where you saved it to)   
        Skip Unattended Installation: Check the box (this allows to install the OS manually)
    3.  The next “Hardware” screen allows you to configure the VM’s memory and CPU Processors (keep in mind that this will be relying on your physical computer’s resources):  
        ![](media/9a91d50faa2f154b1a98c3caf27aa151.png)  
        Just for the installation processed, I increased the Base Memory to 4096 MB (4 Gigs) and increased the CPU to 3 Processors (it just makes it go a whole lot quicker....trust me. If you notice these setting eating too much of you host's resources, by all means lower these.)

        Hit Next

    4.  For the Virtual Hard disk, I decreased to 30 Gigs:  
        ![](media/f964fa4c5d112384d55c9ca2f3990cd1.png)  
          
        Hit Next
    5.  Finally you’ll see a Summary of what you just configured:  
        ![](media/af5ced1d85c6e9332b8ec38be5d05b12.png)  
          
        If you’re good to go, click Finish
    6.  Shortly you should see the new Windows1-Nessus VM in VirtualBox:  
        ![](media/0a2d20fe35497502d40a536a254382be.png)  
          
        Go ahead and hit the green arrow labeled Start at the top.
    7.  Eventually, you’ll be presented with the Windows Setup page:  
        ![A screenshot of a computer Description automatically generated](media/bf82c21e3ff43346a89d10a65591c60f.png)  
        Hit Next
    8.  Hit Install Now:  
        ![A computer screen with a blue background Description automatically generated](media/700d92cb4cf030da1a18f2799f53e96e.png)
    9.  Once you’re presented with Activate Windows screen; select “I don’t have a product key” at the bottom to the left of the Next button:  
        ![A screenshot of a computer Description automatically generated](media/04609678315f37ecd79b61e85df96a62.png)
    10. For the operating system option, select Windows 10 Pro:  
        ![A screenshot of a computer Description automatically generated](media/45000c4485011cb5480f26e82b259235.png)  
        Hit Next
    11. Accept the license terms:  
        ![A screenshot of a computer Description automatically generated](media/fbc54a3f9774e2423e25c7b32fcbeab8.png)  
        Hit Next
    12. Now choose the second option Custom: Install Windows only (advanced)  
        ![A screenshot of a computer Description automatically generated](media/ae06610094ef53173b7cb6c5d7c55dbb.png)
    13. Hit next on the following screen:  
        ![A screenshot of a computer Description automatically generated](media/97abb98c4fc784fa5299aec61ce99462.png)
    14. Now you will see the installation of Windows 10 has started:  
        ![A computer screen shot of a computer screen Description automatically generated](media/c1f762f9563b8023a7ba5f8493ecdca1.png)
    15. Windows will restart automatically to finalize installation  
        ![](media/dae83ec77f90f7734bb24b04722c652c.png)
    16. Choose your region (United States):  
        ![](media/d0f4b915ad82b28d38c3f6d61486dc00.png)
    17. Hit Yes:  
        ![](media/9e37423710e66fed8836b5cd5296b36c.png)
    18. Hit Skip:  
        ![](media/0b53d93bb62f122289257fb9709d5a5f.png)
    19. Select “Set up for personal use” and “Next”:  
        ![](media/34b2674b661028ef51369133f09a858f.png)
    20. Select “Offline account” on bottom left:  
        ![](media/1b4b04c18cfdee093369d419c0e74cef.png)
    21. Then “Limited experience”:  
        ![](media/ab1802816718dcd9a1fdde4b7d812dc7.png)
    22. Input a username and hit Next:  
        ![](media/16fca95e23ca51fdd70ae653e04a99f3.png)
    23. Input a Password and hit Next:  
        ![](media/6797bac89093caf4df5718566eeb2a27.png)
    24. Confirm your Password:  
        ![](media/5963fedefd1bc8dd15d21b7f7e918edc.png)
    25. Answer the Security Questions:  
        ![](media/cb6dde091077f3fbd67ae0e4c195bc00.png)
    26. Select “Not now”:  
        ![](media/0fa4ff57fca804af11a02c6f9d29e8d3.png)
    27. You can do what you like here, but I always toggle these to No:  
        ![](media/a59539ba50e80ae502548767ad357e4a.png)
    28. Hit Skip:  
        ![](media/e9c2c9e744ddcf596f7e7dbb360867ee.png)
    29. Select “Not now”:  
        ![](media/8cd83f4d38eaf406d2914cbe56aadaa8.png)
    30. Finally, we’re up and running:  
        ![](media/c43bb1cd0c8e7852efba135fc127d853.png)

Create a username and password once Windows is done installing:  
  
Username: aaron  
Password: Neverhappened86

