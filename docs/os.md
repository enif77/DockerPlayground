# Operating system - Ubuntu Server 20.04 in VM

1. Download Ubuntu Server 20.04 (OS) from: https://ubuntu.com/download/server
2. Create new 64bit virtual machine (VM) with at least 2 CPUs, 4 GB RAM and 40 GB HDD (dynamically allocated). 
3. Install OS with ssh and no other packages/server modules. Use all default, full disk, keyboard layout of your chice.
4. Create during the OS setup a default user with a name, lets say, `kid` - its a playground, we are building after all, right? Password can be anything. I'll use `Passw0rd1`.
5. Map VM port 22 to a VM external port - I am using 8022.

Internal IP address of the VM can be obtained by executing:

``` 
networkctl status
```

To connect to the newly installed VM run this command:

```
ssh kid@localhost -p 8022
```

TODO: How to setup local time zone.

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