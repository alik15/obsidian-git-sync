How to access local virtual box from the command line(MobaXTerm)

Assign an ip address to your virutual machine while you are installing it

MobaXterm is a terminal emulator for windows to work with servers by connecting to them using ssh
You can also use it to connect to a local virtual machine created using VirtualBox 

For this guide you will need to install 2 things
**1. MobaXterm
2. Virtual box extension pack**

and of course you have to have virtual box installed with a VM setup for testing purposes.
https://mobaxterm.mobatek.net/download.html

# Installing the Virtual Box Extension Pack:

## Check your virtual box version 

If using windows:

**Right-Click** on **VirtualBox Manager** > **Left-Click** on **open file location**. In the File Explorer navigated to **Left-Click doc** > **Left-Click UserManual.pdf

### Download and setup extension pack for your version of VirtualBox 
https://www.virtualbox.org/wiki/Download_Old_Builds
open this link and after selecting your version, look for your specific build and then the extension pack section


### Add extension pack to virtual box
go to preferences>extensions and add the file you downloaded using the + button


Now you should be able to access your virtual machine using mobaxterm 








Go to preferences 
![[Pasted image 20230831110052.png]]

Go to Extensions 

![[Pasted image 20230831110509.png]]

click on the green plus button on the right side, add the extension 



now when you turn your virtual machine on, you can access it through your cli (MobaXTerm) (SSH)
