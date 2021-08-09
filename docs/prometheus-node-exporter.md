# Prometheus - Node Exporter

Node exporter is for monitoring the VM (or a physical machine) itself.

- https://github.com/prometheus/node_exporter
- https://github.com/prometheus/node_exporter/releases
- https://prometheus.io/docs/guides/node-exporter/
- https://prometheus.io/download/#node_exporter
- https://devopscube.com/monitor-linux-servers-prometheus-node-exporter/

## Installation

Check, what is the latest version at the [releases](https://github.com/prometheus/node_exporter/releases) page.

Download and install node exporter:

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
tar xvfz node_exporter-1.2.2.linux-amd64.tar.gz 
sudo cp node_exporter-1.2.2.linux-amd64/node_exporter /usr/local/bin/
```

Create a service user: 

```
sudo useradd -rs /bin/false node_exporter
```

Create a service for the node exporter:

```
sudo nano /etc/systemd/system/node_exporter.service
```

Insert this to the service description file:

```
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter --collector.systemd --collector.processes

[Install]
WantedBy=multi-user.target
```

Reload services daemon and start and enable the new service:

```
sudo systemctl daemon-reload
sudo systemctl start node_exporter

sudo systemctl status node_exporter

sudo systemctl enable node_exporter
```

Node exporter runs at port 9100. You can add its port mapping to VM, so you can access it from outside (OPTIONAL). This is the URL, you can access it on then:

```
http://localhost:9100/
```

Prometheus config:

```
  - job_name: 'node'
    scrape_interval: 5s
    static_configs:
    - targets: ['host.docker.internal:9100']
```

Grafana dashboard:

```
https://grafana.com/grafana/dashboards/1860
```

## Updating

Check, what is the latest version at the [releases](https://github.com/prometheus/node_exporter/releases) page.

```
# Download and extract the latest version:
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
tar xvfz node_exporter-1.2.2.linux-amd64.tar.gz 

# Stop the service:
sudo systemctl stop node_exporter

# Update it:
sudo cp node_exporter-1.2.2.linux-amd64/node_exporter /usr/local/bin/

# Run it again:
sudo systemctl start node_exporter
```

## Postinstall cleanup

You can remove downloaded and extracted node exporter files by:

```
sudo rm -r node_exporter-1.2.2.linux-amd64
rm node_exporter-1.2.2.linux-amd64.*
```

---

## Installing node_exporter to Turris Omnia/OpenWRT

NOTE: You need the `root` user acces to your router enabled to install anything there.

- https://openwrt.org/docs/guide-developer/procd-init-script-example

Download the latest `node_exporter` for the `linux-armv7` platform and copy it to the Turris:

```
scp ./<extracted-archive>/node_exporter root@192.168.1.1:/usr/bin
```

Check, if the `node_exporter` runs (execute this on the router!):

```
/usr/bin/node_exporter
```

You should see log messages from it. Collected metrics should be visible at:

```
http://192.168.1.1:9100/metrics
```

Press CTRL-C to stop it.

### Creating the procd based service for running the node_exporter

Create `node_exporter.service` on your PC with this content:

```
#!/bin/sh /etc/rc.common
USE_PROCD=1
START=95
STOP=01
start_service() {
    procd_open_instance
    procd_set_param command /usr/bin/node_exporter --collector.processes
    procd_set_param stdout 1
    procd_set_param stderr 1
    procd_close_instance
}
```

Copy it to the Turris:

```
scp ./node_exporter.service root@192.168.1.1:/etc/init.d/node_exporter
```

To activate and start the service, run these commands on the Turris:

```
/etc/init.d/node_exporter enable
/etc/init.d/node_exporter start
```

You should see logs from the `node_exporter` here:

```
logread -f
```

Prometheus config:

```
  - job_name: 'node'
    scrape_interval: 5s
    static_configs:
    - targets: ['192.168.1.1:9100']
```
