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
<<p align="center">
<br/>
<img src="https://i.imgur.com/x12bzqc.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
<br /></p>
<br />

## Program walk-through:

#### **Part 1: Installing VirtualBox**

1.  Download VirtualBox at <https://www.virtualbox.org/>
    1.  Click blue Download button
    2.  Choose the platform (Windows hosts for me)
2.  While that is downloading: Checkout the SHA256 checksums link on this page to determine if the file has been tampered with in transit (during the download):  
    <p align="center">
    <br/>
    <img src="https://i.imgur.com/yqIChyh.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    <br />

The SHA256 checksums link will open a new tab with a list of hashes (shown below).<br/>

    - Now the VirtualBox installer is in the Downloads folder:<br/>  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/9ulDx0V.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
        
    - Open PowerShell, go to the downloads folder, type Get-FileHash followed by V then tab (file shows up). Hit enter to generate the SHA256 hash:<br/>  
            <p align="center">
    <br/>
    <img src="https://i.imgur.com/y5oBk7b.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
        
    - Double-click and copy the hash from PowerShell and do a Ctrl-F and paste back in the new tab browser of SHA256 checksum hashes provided by Virtual Box:<br/>  
            <p align="center">
    <br/>
    <img src="https://i.imgur.com/xjJNpbV.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
        
    - As you can see, the hash from PowerShell matches the provided hash. So, we know for a fact that the file has not been tampered with in-transit.<br/>
    <br/>
    <br/>
3.  Now run the exe file from the download folder and follow the install wizard:
4.  Hit Next:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/pgvveld.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    
6.  On the next page (Custom Setup) you can change the location of where VirtualBox is installed (I’m going to accept the defaults and hit Next):  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/yKj2Xh3.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    
8.  The next screen will tell you that it will “reset you network connection and will temporarily disconnect you from the network”. Go ahead and hit Yes if you’re good with that:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/ikCcICC.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    
10.  Hit Yes on the Python Core screen as well:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/ecg3HjB.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>

12.  Select Install:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/v6amPxt.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>

14.  Wait for it to complete:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/AKDykXX.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>

16. When it’s done you’ll see the “Installation Complete” screen:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/lDtR4ZN.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
    
18. Finally hit Finish and VirtualBox will automatically pop on:  
        <p align="center">
    <br/>
    <img src="https://i.imgur.com/ORalQJ2.png" height="80%" width="80%" alt="Microsoft Azure Virtual Machine"/>
    <br /></p>
