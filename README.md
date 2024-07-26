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

1.  Download VirtualBox at <https://www.virtualbox.org/>
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
