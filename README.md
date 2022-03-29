# Docker Playground

My notes about building a playground for Docker using a virtual machine with the Ubuntu Server 20.04 linux.

- [Virtual Box](docs/vm-virtual-box.md)
- [Operating system - Ubuntu Server 20.04](docs/os.md)
- [Operating system - Backup](docs/os-backup.md)
- [Operating system - LVM](docs/os-lvm.md)
- [Docker](docs/docker.md)
- [Docker Compose](docs/docker-compose.md)
- [Portainer](docs/portainer.md)
- [Watchtower](docs/watchtower.md)
- [nginx](docs/nginx.md)
- [Prometheus](docs/prometheus.md)
- [Prometheus - Node Exporter](docs/prometheus-node-exporter.md)
- [Prometheus - Windows Exporter](docs/prometheus-windows-exporter.md)
- [Prometheus - cAdvisor](docs/prometheus-cadvisor.md)
- [Prometheus - nginx Exporter](docs/prometheus-nginx-exporter.md)
- [Grafana](docs/grafana.md)


## MSSQL VM

See [Virtual Box](docs/vm-virtual-box.md), [Ubuntu Server 20.04](docs/os.md), [LVM](docs/os-lvm.md).

* Install Ubuntu server 20.04.x, defaults, with ssh server, no aditional packages.
* Map port 22 to 8022 (or any other) in VirtualBox network settings.

```
sudo update
sudo upgrade

# Turn off sleep:
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target

# Set local timezone:
sudo timedatectl set-timezone Europe/Prague

# Enable ssh access for root:
sudo nano /etc/ssh/sshd_config

# Edit line:
# #PermitRootLogin prohibit-password
# to
# PermitRootLogin yes

sudo systemctl restart ssh

# Set password for root:
sudo bash
passwd

# Root can connect fia SFTP or SSH: ssh root@localhost -p port

# make sure, that the system uses whole disk:
df -h
sudo vgdisplay
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver15

* Install MSSQL 2019 following MS docs.
* Set the `sa` password to `Passw0rd`.
* Map ports 1433 and 1434 in the VirtualBox network settings.

To connect to MSSQL use this command:

```
# To connect from the VM:
sqlcmd -C -S localhost -U SA -P Passw0rd

# To connect from remote computer (outside of the VM):
sqlcmd -C -S 127.0.0.1,1433 -U SA -P Passw0rd

```

* Allow remote admin connections (https://cswsolutions.com/blog/posts/2020/may/connecting-remotely-to-sql-server-on-linux/):

```
sp_configure @configname = 'remote admin connections', @configvalue = 1;
GO
RECONFIGURE;
GO
```

Use whatever MSSQL management tool on the host machine (using the 127.0.0.1 IP address!).


## Multipass

VirtualBox VMs can be created and controlled automatically by the [Multipass](https://multipass.run) tool.

Storage for VMs created by the Multipass can be changed by creating and setting the `MULTIPASS_STORAGE` environment
variable (at system level). Once this variable is set, restart the `Multipass Service` service.

The original path is `C:\Windows\System32\config\systemprofile\AppData\Roaming\multipassd`.

Description for other operating systems is here: [https://github.com/canonical/multipass/pull/1789](https://github.com/canonical/multipass/pull/1789).

How to replace Docker Desktop with Multipass tutorial is here: [https://techsparx.com/](https://techsparx.com/software-development/docker/docker-desktop/multipass.html).


## TODO

* MSSQL - Prometheus Exporter
