# Virtual Box

Download and install the latest version of the Virtual Box - the virtualization platform we are using as a base for the Docker Playground.

- https://www.virtualbox.org/
- https://www.virtualbox.org/wiki/Downloads

## Virtual machine

Create new 64bit virtual machine (VM) with at least 2 CPUs, 4 GB RAM and 40 GB HDD (dynamically allocated). 

### Add a new virtual machine wizard

Choose a name of the VM, its placement on your disk and OS type and variant:

![Name and Operating system](/img/os-new-vm-01.png)

Set the memory size to at least 4096 MB:

![Memory size](/img/os-new-vm-02.png)

We are creating a new VM, so we need a new hard disk now:

![Hard disk](/img/os-new-vm-03.png)

The default VDI hard disk file format is fine:

![Hard disk file type](/img/os-new-vm-04.png)

Use dynamically allocated hard drive, so it wont consume excessive amount of disk space right from the creation of the hard drive. The fixed size hard drive can be more performant, if you need it. 

![Storage on physical hard disk](/img/os-new-vm-05.png)

Here you can change, where you VM's hard drives will be located. The default location is fine. Set the size to at least 40 GB. With dynamically allocated hard drives you can be generous here...  

![File location and size](/img/os-new-vm-06.png)

### Configuration

You can keep all default settings with some exceptions. First - you wont need a virtual Floppy to be connected to your VM:

![System - Motherboard](/img/os-configure-vm-01.png)

Second - the VM should have at least two CPUs:

![System - Processor](/img/os-configure-vm-02.png)

And third - no audio on a server OS is needed:

![Audio](/img/os-configure-vm-03.png)
