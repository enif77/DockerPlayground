# Watchtower

Watchtower is an optional tool, that can watch containers for new versions. It can update containers automatically.

- https://containrrr.dev/watchtower/
- https://github.com/containrrr/watchtower

To install and run Watchtower, use this command:

```
  docker run \
    -d \
    -p 8081:8080 \
    --name watchtower \
    --restart=always \
    --net docker-net \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower \
    --cleanup \
    --rolling-restart \
    --stop-timeout 30s \
    --http-api-metrics \
    --http-api-token Watch123 \
    --schedule "0 0 * * * *"
```

This will run check for updates hourly and with Prometheus metrics exposed. The API token `Watch123` is used by Prometheus to access metrics exposed by Watchtower.

Prometheus config:

```
  - job_name: 'watchtower'
    metrics_path: '/v1/metrics'
    authorization:
      type: bearer
      credentials: 'Watch123'
    static_configs:
    - targets: ['watchtower:8080']
```

NOTE: Dashboard for Grafana is in the GitHub repository of Watchtower.

### Aditional Watchtower run commands

Run  once, but do not update:

```
docker run \
  --name watchtower-check \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --monitor-only \
  --run-once \
  --debug
```

Run once and update:

```
docker run \
  --name watchtower-update \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --run-once \
  --debug
```
