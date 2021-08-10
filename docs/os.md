# Operating system - Ubuntu Server 20.04 in VM

## Operating system

Download **Ubuntu Server 20.04** from: [https://ubuntu.com/download/server](https://ubuntu.com/download/server).

## Install operating system

Install OS with ssh and no other packages/server modules. Use all defaults, full disk, keyboard layout of your choice.

Create during the OS setup a default user with a name, lets say, `kid` - its a playground, we are building after all, right? Password can be anything. I'll use `Passw0rd1`.

![Language selection](/img/os-install-01.png)

![Keyboard layout selection](/img/os-install-02.png)

![Network setup](/img/os-install-03.png)

![Proxy settings](/img/os-install-04.png)

![Ubuntu archive mirror selection](/img/os-install-05.png)

![Hard disk setup](/img/os-install-06.png)

![File system summary](/img/os-install-07.png)

![Profile setup](/img/os-install-08.png)

![SSH setup](/img/os-install-09.png)

![Server modules (snaps) selection](/img/os-install-10.png)

![Install complete!](/img/os-install-11.png)

![First login screen](/img/os-install-12.png)

## Network setup

 Map VM port 22 to a VM external port - I am using 8022. This allows us to connect to our VM via ssh.

Internal IP address of the VM can be obtained by executing:

``` 
networkctl status
```

![Network](/img/os-configure-vm-04.png)

![Port forwarding rules](/img/os-configure-vm-05.png)

To connect to the newly installed VM run this command:

```
ssh kid@localhost -p 8022
```

## Disable sleep and hybernation

- https://linux-tips.us/how-to-disable-sleep-and-hibernation-on-ubuntu-server/
- https://www.unixtutorial.org/disable-sleep-on-ubuntu-server/

To check the status of a sleep service:

```
systemctl status sleep.target
```

Disable all sleep realted services:

```
sudo systemctl mask sleep.target
sudo systemctl mask suspend.target
sudo systemctl mask hibernate.target
sudo systemctl mask hybrid-sleep.target
```

or

```
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

To enable the sleep:

```
sudo systemctl unmask sleep.target suspend.target hibernate
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
