# Prometheus

- https://prometheus.io/
- https://prometheus.io/docs/prometheus/latest/installation/
- https://stackoverflow.com/questions/62040337/where-does-prometheus-store-metrics-data-in-container
- https://stackoverflow.com/questions/31324981/how-to-access-host-port-from-docker-container
- https://stackoverflow.com/questions/51425935/nginx-proxypass-to-prometheus-using-variable
- https://www.robustperception.io/using-external-urls-and-proxies-with-prometheus
- https://www.robustperception.io/external-urls-and-path-prefixes

Create a named volumes for storing Prometheus data:

```
docker volume create prometheus_config
docker volume create prometheus_data
```

Download and run Prometheus:

```
docker run \
  -d \
  -i -t \
  -p 9090:9090 \
  --name=prometheus \
  --restart=always \
  --net docker-net \
  --add-host=host.docker.internal:host-gateway \
  -v prometheus_config:/etc/prometheus \
  -v prometheus_data:/prometheus \
  prom/prometheus \
  --web.enable-lifecycle \
  --web.route-prefix=/ \
  --web.external-url=http://localhost:5000/prometheus \
  --config.file=/etc/prometheus/prometheus.yml
```

This command will deploy Prometheus with monitoring API and with support for nginx proxy.

Configuration file can be edited by:

```
  sudo nano /var/lib/docker/volumes/prometheus_config/_data/prometheus.yml
``` 

To trigger Prometheus to reload its `prometheus.yml` config file:

```  
curl -X POST localhost:9090/-/reload
```

If you wan to access Prometheus from outside of the VM, add port 9090 network mapping there.

URL for accessing Prometheus:

```
http://localhost:9090/
```
