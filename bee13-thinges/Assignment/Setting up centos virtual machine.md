In this assignment you will be learning about virtual machines and then setting up one on your personal PC

## What is a virtual machine anyway ?

A virtual machine is an emulation of a physical computer. It exists as code but can do perform computation just like a computer.
 A virtual machine "borrows" CPU and RAM from a computer/remote server. This makes it possible to install operating systems on it just like a normal PC
A "side effect" of virtualization is that one can run a virtual machine *inside* a virtual machine.
You can learn more about Virtualization and VMs using the following link:

https://www.ibm.com/topics/virtual-machines
## Why do we use virtual machines ?

Virtual machines can run multiple operating systems on the same physical machine *at the same time*. This allows us to run multiple softwares and applications that can only run on a few operating systems. 
It also allows you to fully utilize your physical system since a single program doesn't utilize all of the physical system, one can run multiple virtual machines on a single physical server to run multiple programs.

### Hypervisors
Hypervisors are the actual programs that can create and run virtual machines. They  do the part of talking to your hardware/host machine OS to provide the virtual resources. 

We will be using virtual box as our hypervisor, Install it from the following link:
https://www.virtualbox.org/wiki/Downloads

You will also need an image of an operating system. For future purposes download the centos image from the following link:
http://centos.mirror.net.in/centos/7.9.2009/isos/x86_64/
![[Pasted image 20240131184629.png]]

Download the one with the name:

`CentOS-7-x86_64-Minimal-2009.iso`



<div style="page-break-after: always; visibility: hidden">
\pagebreak
</div>

## Setting up the Virtual Machine
Once installed, open Virtualbox.
![[Pasted image 20240131193102.png]]

I have a few virtual machines setup, yours will be empty. 


To start click on new and give your VM a name
- set ram to 2 GBs (2048MBs)
- In the virtual harddisk section dont change anything and keep continuing until you reach the screen asking you to select a size for your virtual machine, set it to 16GBs or above 


<div style="page-break-after: always; visibility: hidden">
\pagebreak
</div>

## Mounting CentOS image

We have created a virtual machine but there is no operating system in it. we have to add an installer image to this virtual machine from which to install. We will be using CentOS image we downloaded earlier.

When u select the virtual machine u just made, the gear icon in the horizontal menu at the top will become visible.. Go to the storage section and click on the cd icon with a plus symbol on it.

![[Pasted image 20240131201041.png]]

Here you can add the ISO image you downloaded for CentOS. Once this is done we will move onto the next step. 


## Installing CentOS on our VM 

Back at the Virtual Box menu click on start to start your virtual machine

Select Install CentOS 7:

![[Pasted image 20240127005806.png]]

![[Pasted image 20240127005951.png]]


We need to focus on the two sections:

1. Installation Destination 
2. Network and Hostname



### 1. Installation Destination

![[Pasted image 20240127010103.png]]
You dont have to do anything here, just open the installation destination and then close it by clicking done.

![[Pasted image 20240127010314.png]]



![[Pasted image 20240127153744.png]]

### 2. Network and Hostname

![[Pasted image 20240127010344.png]]


>[!Warning]
>The following part is very important as you wont be able to do this later if u skip this step.

Once you open Network and hostname, modify nothing except making sure to TURN ON the ethernet switch as shown in the above picture.


## Root Password

After you click the installation button you will be greeted with a root password and username making screen
![[Pasted image 20240127154503.png]]

setup a root password and finish the installation.


When the installation is finally complete, you should turn it off, remove the installation medium: 
settings>storage>right click on the image and click remove attachment




## Setting up a static IP Address

This section is important for later 
This should be what the networking settings should look like

![[Pasted image 20240127160048.png]]

![[Pasted image 20240127160144.png]]

## The better way to setup a virtual IP for CentOS
## Testing the Virtual Machine 

You can login to the account you made

to test the cent-os installation run the following command 
```bash 
ping google.com
```



You can go ahead and start exploring linux through the command line such as learning to use the vi editor: 

https://www.howtogeek.com/102468/a-beginners-guide-to-editing-text-files-with-vi/
https://ss64.com/vi.html

or exploring the CentOS itself:

https://webhostinggeeks.com/blog/what-is-centos-beginners-guide-centos-linux-distro/