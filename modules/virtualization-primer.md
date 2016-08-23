# Virtualization

### Cybersecurity First Principles
* __Domain Separation__: Good fences make good neighbors. When trying to secure a home or computer, separating the areas where resources are and people work prevents accidents and loss of data or private information. We are preventing the information worlds from colliding. In this lesson, we apply this principle to managing distinct sets of resources by seperating them with a virtual fence in a 'virtual machine.'

* __Resource Encapsulation__: A resource can be hardware such as memory, disk drives, or a display screen. It can also be system objects such as semaphores, a linked list, or shared memory. Processes (or programs) need resources to run. Resources have to be separated and used in the way they were intended. Virtual machines encapsulate an operating system and all of the processes and applications it contains.

* __Process Isolation__: A process is when a program is run. By keeping processes separated, it prevents the failure of one process from causing another to fail. Virtual machines isolate processes running inside them from those running elsewhere on a computer. This allows users to do many dangerous tasks (such as malware analysis) with little or no risk of harm outside of the virtual machine.


### Table of Contents
[Introduction](#introduction)  
[Installing VirtualBox](#installing-virtualbox)  
[Installing Ubuntu Desktop Linux VM](#installing-ubuntu-desktop-linux-vm)  
[Customizing the VM](#customizing-the-vm)  
[Installing Software in the VM](#installing-software-in-the-vm)   
[Taking Snapshots](#taking-snapshots)  
[Installing Ubuntu Server Linux VM](#installing-ubuntu-server-linux-vm)   
[Launch an existing VM](#launch-an-existing-vm)  
[Further Readings](#further-readings)  


### Introduction

Virtualization is a technique to abstract computer hardware resources. It allows sharing resources like CPU, memory and input/output devices between many __guest__ Operating Systems (OS). A host OS with virtualization software manages many guest OSes. Through virtualization a guest OS is unaware of other guest OSes and the sharing of underlying physical computing resources with them. Virtualization provides _domain separation_ among guest OSes as well as with the host OS. A guest OS is also referred to as a __virtual machine__ or VM for short.

Virtualization technologies are essential to the operation of large scale data centers. It allows many tenants to share the same data center resources. Virtualization enables them to do so without encroaching on each other's data and programs. This is an example of __resource encapsulation__.

In this module we will install virtualization software on a Windows host OS. Next, install a Ubuntu Linux guest OS as a VM. This setup will provide __domain separation__ between our host OS and the development environment. Any changes and/or "accidents" in the guest OS, stay contained within the VM. Virtualization also allows safe experimentation with unknown software programs and malware samples. This is an example of __process isolation__. The failure of processes in a VM due to malware infection, does not impact the processes in the host machine. The VM may freeze, but it does not tie up the host machine resources.

Our entire setup uses Free and Open Source Software (FOSS). When using FOSS, respect its copyright and license restrictions. These obligations and rights are typically conveyed in a LICENSE file.

[Top](#table-of-contents)

### Installing VirtualBox

VirtualBox is a free open source virtualization software from Oracle. For a Windows host OS, a installation executable can be downloaded from here:

> If you see a VirtualBox shortcut on your lab computer desktop, you may skip the download and installation and go on to the [next step](#installing-ubuntu-desktop-linux-vm).

```text
https://www.virtualbox.org/wiki/Downloads
```

Click on the `VirtualBox xx.xx.xxx for Windows hosts  x86/amd64` download link. Once the file is downloaded, proceed with installation by double clicking the executable. Continue with defaults and answer YES to any prompts. Installation will require an account with administrative privileges.

Upon successful installation, you should see this.

> On your lab machine there may be some VMs already configured for you. So your interface may look slightly different.

![start](virtualization-img/0-vboxstartclean.png)

[Top](#table-of-contents)

### Installing Ubuntu Desktop Linux VM

For our guest OS we will select a popular Linux distribution. The Ubuntu Desktop OS. It is easy to setup and supports many development environments. In particular, the 64-bit Ubuntu version 14.04, Trusty Thar, is supported by many open source software (OSS) packages and development frameworks.

> Fact: Facebook capture the flag hosting package found at `https://github.com/facebook/fbctf` only supports installation on Ubuntu 14.04 OS. This shows how developer friendly Ubuntu Linux operating systems are.

To create a new VM, press the blue `New` button in VirtualBox.
>![start](virtualization-img/0-virtualboxstart.png)

It should bring up a `Create Virtual Machine` prompt.
>![vminstall](virtualization-img/1-selectos.png)

Go ahead and enter a `Name:` like `Dev Machine`  
When you list the `Type:` option, you will see that VirtualBox supports many types of guest OSes. Select `Linux`.  
For `Version:` select `Ubuntu (64-bit)`  

> Its possible, depending on which host OS you are using, that your operating system may only support 32-bit virtual machines - in which case you will need to use a [32-bit Ubuntu Desktop install](http://mirror.pnl.gov/releases/trusty/ubuntu-14.04.4-desktop-i386.iso). Its also possible, that your BIOS is not configured to support 64bit VMs - in which case you can follow the easy guide at [https://techfixes.net/how-to-fix-virtualbox-only-showing-32-bit-option/](https://techfixes.net/how-to-fix-virtualbox-only-showing-32-bit-option/) to resolve this issue.

Your final configuration should look like this. Then click `Next`.

>![vminstall](virtualization-img/2-selectos.png)

Now adjust the amount of RAM available to the VM. 2GB (or 2048 MB) is good, but if your physical machine does not have a lot of RAM then leave it at 768 MB. My machine has 8 GB RAM, so I had plenty of room to spare. Click `Next`.

>![vminstall](virtualization-img/3-memorysize.png)

The next configuration step creates a virtual hard drive for the VM. Select the `Create a virtual hard disk now` option. Then click `Create`.

>![vminstall](virtualization-img/4-harddisk.png)

On the next `Hard disk file type` prompt, change the selection to **VMDK**. This hard drive format plays nice with `VMware`, another popular virtualization software. This hard drive format is also compatible with the Open Virtualization Format (OVF). OVF allows seamless export of VMs between different virtualization environments. Click `Next`.

>![vminstall](virtualization-img/5-vmdktype.png)

To optimize storage, select the `Dynamically allocated` option on the `Storage on physical hard disk` prompt. Click `Next`.

>![vminstall](virtualization-img/6-harddiskallocation.png)

Finally, in the `File location and size` prompt specify the name of the new hard drive and select a location to store it. On your physical machine, the hard drive appears as a single, large VMDK file. Click `Create`.

> ![vminstall](virtualization-img/7-filelocation.png)

The _Dev Machine_ VM is now ready to be started in VirtualBox. It is currently powered off. Now before you start this VM, we need a installation CD for the Ubuntu Desktop OS.

Download the ISO file for Ubuntu 14.04 64-bit Desktop version from here. It is a large download but it includes all the files needed for installation.
> On your host computer check C:/ISO/Desktop folder to see if this file is already on your computer. If yes, skip the download step.

```text
http://releases.ubuntu.com/trusty/ubuntu-14.04.4-desktop-amd64.iso

```
Save the ISO file in a convenient location on your computer.  

Now hit the Start button for the **Dev Machine** VM in VirtualBox.
> ![vminstall](virtualization-img/8-startvm.png)

A `Select start-up disk` prompt should appear. Click the Folder Icon with a green pointer to select the ISO file as our start-up disk.
> ![vminstall](virtualization-img/9-startup-disk.png)

Browse to the location where you saved the Ubuntu ISO file.
> On the lab computer, it will be in C:/ISO/Desktop

Then select the ISO file as the start-up disk. Now click `Start`.
> ![vminstall](virtualization-img/10-ubuntuiso.png)

The VM will now start with a boot screen displayed.

As you interact with the VM window, you will notice that it captures your mouse pointer. To release the mouse pointer you have to press the **host key** on your keyboard. Upon clicking in the VM window, a information dialog box describes how the host key works. In this case, the host key is the **Right Alt** key. Once you are aware of the host key, select the `Do not show this message again` and click `Capture`. To return to your host OS, press the host key to release your mouse from the VM window.

> ![vminstall](virtualization-img/11-host-key.png)

Now click anywhere in the VM window for it to capture the mouse once again. Then click on the `Install Ubuntu` button.

> ![vminstall](virtualization-img/12-installubuntu.png)

Click `Continue` on the next prompt.

> ![vminstall](virtualization-img/13-installubuntu.png)

The next prompt warns you about `Erase disk and install Ubuntu`. It is OK to continue as this action will be carried out on the new virtual hard drive. It does not impact your host OS hard drive. Click `Install Now`.

> ![vminstall](virtualization-img/14-installubuntu.png)

Click `Continue` on the next few prompts.

> ![vminstall](virtualization-img/15-installubuntu.png)

> ![vminstall](virtualization-img/16-installubuntu.png)

On the `Who are you?` prompt, enter account details. Since this is a sacrificial Dev Machine. Setup a generic account name. Remember these credentials to login to the machine later. Click `Continue`.
> ![vminstall](virtualization-img/17-setupaccount.png)

After a few more prompts, the installation should be completed.
> ![vminstall](virtualization-img/18-installubuntu.png)

Click `Restart Now` to restart the VM.

> ![vminstall](virtualization-img/19-restartubuntu.png)

Hit `ENTER` to complete restarting and finish installation.
> ![vminstall](virtualization-img/20-restartubuntu.png)


### Customizing the VM

Upon restarting after installation, you should see a login prompt. Login with the account credentials setup during installation.

> ![vminstall](virtualization-img/21-login.png)

For development, you will be working a lot in the `Terminal` app. To start it, click the Ubuntu home icon and type "Terminal". Select the Terminal app from the search results.
> ![vminstall](virtualization-img/22-terminal.png)

A terminal app will now appear. In the terminal type the `ls` command to list the directories in your home folder on the VM. This directory structure is completely separate from your home folder on the host OS.
> ![vminstall](virtualization-img/23-lscmd.png)

You will probably notice that the screen resolution for the VM window is not very user-friendly. However, just resizing the window does not change the screen resolution of the Desktop VM. To enable screen resizing and other features we need to install Guest Additions in the VM. To do so, select the `Insert Guest Additions CD image...` menu option as shown below.
> ![vminstall](virtualization-img/24-guest-additions.png)

Click `Run` on the next prompt.
> ![vminstall](virtualization-img/25-guest-additions.png)

To proceed with the installation, enter your account password.
> ![vminstall](virtualization-img/26-guest-additions-pw.png)

After a successful install you should see a terminal window like this. Press `ENTER` to close this terminal window.
>![vminstall](virtualization-img/27-guest-additions-done.png)

Now, restart the machine by clicking on the power button in the upper right-hand corner. Then select the `Restart` option.
> ![vminstall](virtualization-img/28-guest-additions-restart.png)

After a restart, resize the Desktop to a comfortable resolution by just resizing the VM window. You may also enter Full Screen Mode, by pressing `Alt + F`. You can exit full screen mode by pressing `Alt + F` again.

Now start the terminal app as shown before and pin it to the Dock. You can accomplish this by right clicking the dock icon and selecting `Lock to Launcher` option. This shortcut will come in handy for invoking terminals for development related tasks.
> ![vminstall](virtualization-img/29-pin-terminal.png)

[Top](#table-of-contents)

### Installing Software in the VM

After a fresh OS install, it is best practice to check for the latest patches and security updates. Issue the following commands in a terminal.

```bash
sudo apt-get update
sudo apt-get upgrade
```

Next, install version control tools. We will use them in the next module.

```bash
sudo apt-get -y install git
```
[Top](#table-of-contents)

### Taking Snapshots

Now that we have the VM setup just like we want, it would be prudent to save this "pristine" state. Saving a state allows us to revert back to it incase something breaks or we suspect a malware infection in the VM.

Follow the tutorial steps in the VirtualBox [user manual](https://www.virtualbox.org/manual/ch01.html#snapshots) and take a snapshot of your Ubuntu Desktop VM and then restore your VM to that snapshot. We will use this functionality several times during the camp.

[Top](#table-of-contents)

### Installing Ubuntu Server Linux VM

Using what you learned here, now install a Ubuntu Server VM. You may download the ISO for 64-bit Ubuntu Server OS version 14.04 from here.
> Optional: Check C:/ISO/Server folder to see if this ISO file is already on your computer

```text
http://releases.ubuntu.com/trusty/ubuntu-14.04.4-server-amd64.iso

```
> Its possible, depending on which host OS you are using, that your operating system may only support 32-bit virtual machines - in which case you will need to use a [32-bit Ubuntu server install](http://mirror.pnl.gov/releases/trusty/ubuntu-14.04.4-server-i386.iso). Its also possible, that your BIOS is not configured to support 64bit VMs - in which case you can follow the easy guide at [https://techfixes.net/how-to-fix-virtualbox-only-showing-32-bit-option/](https://techfixes.net/how-to-fix-virtualbox-only-showing-32-bit-option/) to resolve this issue.

You do not need to install Guest Additions for a Server VM, since it only has terminal based interactions.

### Launch an existing VM

VirtualBox also allows launching VMs that others may have created. We will use this feature to load a previously configured development environment.

> Check C:/ISO/GenCyber folder for files with `.ovf` or `.ova` extension. If such files exist, then from VirtualBox select the `File --> Import Appliance` menu option to import them.

> ![vminstall](virtualization-img/30-importappliance.png)

This will bring up the `Import Virtual Appliance` prompt.

> ![vminstall](virtualization-img/31-importappliance.png)

Navigate to the folder with the `.ovf` or `.ova` files, select one and click `Open`. Follow the prompts to import the VM and start it. The imported VM will be your development environment for the next several modules.

[Top](#table-of-contents)

### Further Readings

* VirtualBox [User Manual](https://www.virtualbox.org/manual/UserManual.html). This user manual has a lot more information on VMs including network settings.
* [Shell prompt basics for a Linux Based OS](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/4/html/Step_by_Step_Guide/ch-basics.html), Redhat. I highly recommend going through this to be a command-line ninja!

> #### _Security tip_
Do not allow VMs to share folders and storage volumes with other VMs or with the host OS. This prevents unintended data sharing between separated domains.

[Top](#table-of-contents)

## Special Thanks

* A special thanks to Matt Hale, Aaron Vigal and Cade Wollcot for reviews of this module and thoughtful discussions.

[Top](#table-of-contents)

#### License
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Cybersecurity Modules</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://faculty.ist.unomaha.edu/rgandhi/" property="cc:attributionName" rel="cc:attributionURL">Robin Gandhi</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

### Modifications
This content has been modified slightly for the Secure Web Development class by Matt Hale.