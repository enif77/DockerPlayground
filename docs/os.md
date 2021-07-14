# Operating system - Ubuntu Server 20.04 in VM

## Operating system

Download **Ubuntu Server 20.04** from: [https://ubuntu.com/download/server](https://ubuntu.com/download/server).

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

## Install operating system

Install OS with ssh and no other packages/server modules. Use all defaults, full disk, keyboard layout of your choice.

Create during the OS setup a default user with a name, lets say, `kid` - its a playground, we are building after all, right? Password can be anything. I'll use `Passw0rd1`.

## Network setup

 Map VM port 22 to a VM external port - I am using 8022. This allows us to connect to our VM via ssh.

Internal IP address of the VM can be obtained by executing:

``` 
networkctl status
```

To connect to the newly installed VM run this command:

```
ssh kid@localhost -p 8022
```

## How to setup local time zone

https://linuxize.com/post/how-to-set-or-change-timezone-on-ubuntu-20-04/

To check, what is your current timezone:

```
timedatectl
ls -l /etc/localtime
cat /etc/timezone
```

Get the list of timezones:

```
timedatectl list-timezones
```

To set a timezone:

```
sudo timedatectl set-timezone Europe/Prague
```

Verify the new settings:

```
timedatectl
```

## SSH access for the user root

More info here: https://linuxconfig.org/allow-ssh-root-login-on-ubuntu-20-04-focal-fossa-linux

```
sudo nano /etc/ssh/sshd_config
```

Find and edit line:

```
#PermitRootLogin prohibit-password
```

to:

```
PermitRootLogin yes
```

Restart the ssh service:

```
sudo systemctl restart ssh
```

To set a password for the user `root` run this:

```
sudo bash
passwd
```

Connect as the user `root`:

```
ssh root@localhost -p port
```

NOTE: Using the user `root` to connect to your VM (for example with [WinSCP](https://winscp.net/)) makes life much easier - you can go to whatever directory you want - but the VM becomes **MUCH LESS SECURE**! Do not use the user `root` to access a real server.

If I am doing setup or maintenance on the VM, I am using the user `kid` with `sudo`. I am using the user `root` to access system and Docker files/volumes via WinSCP only.

## OS Maintenance

```
sudo apt update
sudo apt upgrade
```

Its good to have backup of the `/etc` folder, before any changes to configuration files in it.

## Accessing VM files

To access internal files of the VM, you can use [WinSCP](https://winscp.net/) or any other ssh ready program. No need to use FTP, SMB or any other protocol.