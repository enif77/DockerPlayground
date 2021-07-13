# Prometheus - nginx Exporter

nginx exporter is for monitoring nginx.

- https://github.com/nginxinc/nginx-prometheus-exporter
- https://nginx.org/en/docs/http/ngx_http_stub_status_module.html
- https://www.datadoghq.com/blog/how-to-collect-nginx-metrics/

Download and run nginx exporter:

```
docker run \
  -d \
  -p 9113:9113 \
  --name portal-metrics-exporter \
  --restart=always \
  --net docker-net \
  nginx/nginx-prometheus-exporter \
  -nginx.scrape-uri=http://portal:80/nginx_status
```

Metrics provided by the nginx exporter can be checked at: 

```
http://localhost:5000/nginx_status
```
or by wget:

```
wget -SO- http://portal:80/nginx_status
```

Prometheus config:

```
  - job_name: 'nginx-portal'
    metrics_path: '/metrics'
    static_configs:
    - targets: ['portal-metrics-exporter:9113']
```
