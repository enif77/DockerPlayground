# Prometheus - cAdvisor

cAdvisor is for monitoring Docker.

- https://github.com/google/cadvisor
- https://prometheus.io/docs/guides/cadvisor/

Download and run cAdvisor:

```
sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --restart=always \
  --detach=true \
  --name=cadvisor \
  --privileged \
  --device=/dev/kmsg \
  --net docker-net \
  gcr.io/cadvisor/cadvisor
```

Grafana dashboards:

- https://grafana.com/grafana/dashboards/11600
- https://grafana.com/grafana/dashboards/193

Prometheus config:

```
  - job_name: 'cadvisor'
    scrape_interval: 5s
    static_configs:
    - targets: ['host.docker.internal:8080']
```
