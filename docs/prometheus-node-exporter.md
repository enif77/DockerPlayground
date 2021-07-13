# Prometheus - Node Exporter

Node exporter is for monitoring the VM itself.

- https://github.com/prometheus/node_exporter
- https://prometheus.io/docs/guides/node-exporter/
- https://prometheus.io/download/#node_exporter
- https://devopscube.com/monitor-linux-servers-prometheus-node-exporter/

Download and install node exporter:

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.1.2/node_exporter-1.1.2.linux-amd64.tar.gz
tar xvfz node_exporter-1.1.2.linux-amd64.tar.gz 
sudo cp node_exporter-1.1.2.linux-amd64/node_exporter /usr/local/bin/
```

Create a service user: 

```
sudo useradd -rs /bin/false node_exporter
```

Create a service for node exporter:

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
ExecStart=/usr/local/bin/node_exporter

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