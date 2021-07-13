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

  - job_name: 'cadvisor'
    scrape_interval: 5s
    static_configs:
    - targets: ['host.docker.internal:8080']

 ## Additional links

- https://prometheus.io/docs/prometheus/latest/installation/
- https://github.com/prometheus/blackbox_exporter/blob/master/example.yml#L19
- https://dev.to/ablx/minimal-prometheus-setup-with-docker-compose-56mp
- https://stackoverflow.com/questions/24319662/from-inside-of-a-docker-container-how-do-i-connect-to-the-localhost-of-the-mach
- https://docs.docker.com/storage/volumes/
- https://valyala.medium.com/how-to-use-relabeling-in-prometheus-and-victoriametrics-8b90fc22c4b2
