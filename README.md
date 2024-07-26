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

![A diagram of a computer network Description automatically generated](media/026eb62ec70cb8745075b1bec3a97f95.png)

## Program walk-through:

#### **Part 1: Installing VirtualBox**

1.  Download VirtualBox at <https://www.virtualbox.org/>
    1.  Click blue Download button
    2.  Choose the platform (Windows hosts for me)
2.  While that is downloading: Checkout the SHA256 checksums link on this page to determine if the file has been tampered with in transit (during the download):  
    ![A screenshot of a computer Description automatically generated](media/ce5445489c9aebab53c4e5939a57d548.png)  
    The SHA256 checksums link will open a new tab with a list of hashes (shown below).
    1.  Now the VirtualBox installer is in the Downloads folder:  
        ![A screenshot of a computer Description automatically generated](media/ba71928412a9bf5dfc3cd0c9beb84b80.png)
    2.  Open PowerShell, go to the downloads folder, type Get-FileHash followed by V then tab (file shows up). Hit enter to generate the SHA256 hash:  
        ![A blue screen with white text Description automatically generated](media/d4dc69ed52a2680d3ebf4e4044d51419.png)
    3.  Double-click and copy the hash from PowerShell and do a Ctrl-F and paste back in the new tab browser of SHA256 checksum hashes provided by Virtual Box:  
        ![A screenshot of a computer Description automatically generated](media/2b8623ab48f9a69a04f61907372496ec.png)
    4.  As you can see, the hash from PowerShell matches the provided hash. So, we know for a fact that the file has not been tampered with in-transit.
3.  Now run the exe file from the download folder and follow the install wizard:
4.  Hit Next:  
    ![A screenshot of a computer Description automatically generated](media/b3e2b5e183ea96c0f8a11da3d5a881e2.png)
5.  On the next page (Custom Setup) you can change the location of where VirtualBox is installed (I’m going to accept the defaults and hit Next):  
    ![A screenshot of a computer Description automatically generated](media/5af70316b204680f50b5372cc658a221.png)
6.  The next screen will tell you that it will “reset you network connection and will temporarily disconnect you from the network”. Go ahead and hit Yes if you’re good with that:  
    ![A screenshot of a computer Description automatically generated](media/b5377529decd45e1697cda9e374eb493.png)
7.  Hit Yes on the Python Core screen as well:  
    ![A box with a logo on it Description automatically generated](media/a102ffc8073c4d96375e762bef4fe4b4.png)
8.  Select Install:  
    ![A screenshot of a computer Description automatically generated](media/99f31370a31e74320b0dc3dd8468b706.png)
9.  Wait for it to complete:  
    ![A screenshot of a computer Description automatically generated](media/ebdb5cfb9d9469808a8f2b0bd7bd8964.png)
10. When it’s done you’ll see the “Installation Complete” screen:  
    ![A screenshot of a computer Description automatically generated](media/f3b033494cc120b51ca8813ae34c9e15.png)
11. Finally hit Finish and VirtualBox will automatically pop on:  
    ![A screenshot of a computer Description automatically generated](media/56f20a3961fbb2526994325be629fa5a.png)
