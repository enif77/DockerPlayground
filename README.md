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


## Multipass

VirtualBox VMs can be created and controlled automatically by the [Multipass](https://multipass.run) tool.

Storage for VMs created by the Multipass can be changed by creating and setting the `MULTIPASS_STORAGE` environment
variable (at system level). Once this variable is set, restart the `Multipass Service` service.

The original path is `C:\Windows\System32\config\systemprofile\AppData\Roaming\multipassd`.

Description for other operating systems is here: [https://github.com/canonical/multipass/pull/1789](https://github.com/canonical/multipass/pull/1789).


## TODO

* MSSQL
* MSSQL - Prometheus Exporter
