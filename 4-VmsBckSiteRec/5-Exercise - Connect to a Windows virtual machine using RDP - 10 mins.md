# Exercise - Connect to a Windows virtual machine using RDP

_Recording of exercise review_
_https://siriusazureciestg.blob.core.windows.net/videos/section4/5-Exercise-Recording.mp4_

* 10 minutes

You have your Windows VM deployed and running, but it's not configured to do any work.

There are a few things we would need to configure to support this scenario:

* Install the FTP server role.
* Prepare the data disk to receive data.
* Install third-party software.

Many of these are typical administrative tasks we won't actually cover here, and we don't have software to install. Instead, we will walk through the steps and show you how you _could_ install custom or third-party software using Remote Desktop. Let's start by getting the connection information.

## Connect to the VM with Remote Desktop Protocol

To connect to an Azure VM with an RDP client, you will need:

* The public IP address of the VM (or private if the VM is configured to connect to your network).
* The port number.

You can enter this information into the RDP client, or download a pre-configured **RDP** file.

**Note**
An **RDP** file is a text file that contains a set of name/value pairs that define the connection parameters for an RDP client to connect to a remote computer using the Remote Desktop Protocol.

### Download the RDP file

1. In the [Azure portal](https://portal.azure.com), ensure the **Overview** panel for the virtual machine that you created earlier is open. You can find the VM under **All Resources** if you need to open it. The overview panel has a lot of information about the VM.

    * You can see whether the VM is running.
    * Stop or restart it.
    * Get the public IP address to connect to the VM.
    * See the activity of the CPU, disk, and network.
2. Click the **Connect** button at the top of the pane.

3. In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings, then click **Download RDP File** and save it to your computer.

4. Before we connect, let's adjust a few settings. On Windows, find the file using Explorer, right-click and select **Edit**. On MacOS you will need to open the file first with the RDP client and then right-click on the item in the displayed list and select **Edit**.

5. You can adjust a variety of settings to control the experience in connecting to the Azure VM. The settings you will want to examine are:

    * **Display**: By default, it will be full screen. You can change this to a lower resolution, or use all your monitors if you have more than one.
    * **Local Resources**: You can share local drives with the VM - allowing you to copy files from your PC to the VM. Click the **More** button under **Local devices and resources** to select what is shared.
    * **Experience**: Adjust the visual experience based on your network quality.
6. Share your Local C: drive so it will be visible to the VM.

7. Switch back to the **General** tab and click **Save** to save the changes. You can always come back and edit this file later to try other settings.

### Connect to the Windows VM

1. Click the **Connect** button to start the connection to the VM.

2. In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.

3. In the **Windows Security** dialog box, enter your username and password that you used in steps 6 and 7.

    **Note**

    If you are using a Windows client to connect to the VM, it will default to known identities on your machine. You can click the **More choices** option and select "Use a different account" to let you enter a different username/password combination.

4. In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.

### Install worker roles

The first time you connect to a Windows server VM, it will launch Server Manager. This allows you to assign a worker role for common web or data tasks. You can also launch the Server Manager through the Start Menu.

This is where we would add the Web Server role to the server. This will install IIS and as part of the configuration you would turn off HTTP requests and enable the FTP server. Or, we could ignore IIS and install a third-party FTP server. We'd then configure the FTP server to allow access to a folder on our big data drive we added to the VM.

Since we aren't going to actually configure that here, just close Server Manager.

## Install custom software

We have two approaches we can use to install software. First, this VM is connected to the Internet. If the software you need has a downloadable installer, you can open a web browser in the RDP session, download the software, and install it. Second, if your software is custom - like our custom service, you can copy it from your local machine over to the VM to install it. Let's look at this latter approach.

1. Open File Explorer. Click on **This PC** in the sidebar. You should see several drives:

    * Windows (C:) drive representing the OS.
    * Temporary Storage (D:) drive.
    * Your local C: drive (it will have a different name than shown below).

    ![Screenshot showing the local drive shared with the Azure VM.](images/connectwinrdp1.png)

With access to your local drive, you can copy the files for the custom software onto the VM and install the software. We won't actually do that since it's just a simulated scenario, but you can imagine how it would work.

The more interesting thing to observe in the list of drives is what is _missing_. Notice that our **Data** drive is not present. Azure added a VHD but didn't initialize it.

## Initialize data disks

Any additional drives you create from scratch will need to be initialized and formatted. The process for doing this is identical to a physical drive.

1. Launch the **Disk Management** tool from the Start Menu. You may have to go to the Computer Management tool first, then Disk Management, or try searching for "Disk Management" in the Start Menu.

2. It will display a warning that it has detected an uninitialized disk.

    ![Screenshot showing the disk management tool warning about an uninitialized data disk in the VM.](images/connectwinrdp2.png)

3. Click **OK** to initialize the disk. It will then show up in the list of volumes where you can format it and assign a drive letter.

4. Open File Explorer and you should now see your data drive.

5. Go ahead and close the RDP client to sign out of the VM. The server will continue to run.

RDP allows you to work with the Azure VM just like a local computer. With Desktop UI access, you can administer this VM as you would any Windows computer: installing software, configuring roles, adjusting features and other common tasks. However, it's a manual process - if we always need to install some software, you might consider automating the process using scripting.
