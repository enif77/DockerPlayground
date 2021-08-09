# nginx

nginx is used in proxy server mode, so we can access multiple internal VM services without a need to expose their ports.

- https://nginx.org
- https://nginx.org/en/docs/beginners_guide.html
- https://hub.docker.com/_/nginx

Create a named volumes for storing nginx data and configuration:

```
docker volume create nginx_config
docker volume create nginx_data
```

Download and run nginx:

```
docker run \
  -d \
  -p 5000:80 \
  -p 8090:8080 \
  --name portal \
  --restart=always \
  --net docker-net \
  -v nginx_config:/etc/nginx:ro \
  -v nginx_data:/usr/share/nginx/html:ro \
  nginx
```

`nginx` should contain module for publishing metrics. You can test it by running following command inside of its container:

```
nginx -V 2>&1 | grep -o with-http_stub_status_module
```
The configuration file can be edited by:

```
sudo nano /var/lib/docker/volumes/nginx_config/_data/nginx.conf
```

My `nginx.conf` looks like this:

```
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {

    fastcgi_buffers 16 16k;
    fastcgi_buffer_size 32k;
    proxy_buffer_size   128k;
    proxy_buffers   4 256k;
    proxy_busy_buffers_size   256k;
    large_client_header_buffers 4 64k;

    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    # This will break the proxy setup!
    #include /etc/nginx/conf.d/*.conf;

  server {

      # this is the internal Docker DNS, cache only for 30s
      resolver 127.0.0.11 valid=30s;

      root /usr/share/nginx/html;
      index index.html index.htm;

      location = /nginx_status {
        stub_status;
      }

      # Uncomment, what you have installed. nginx wont start, if something below does not exists!

      #location /portainer/ {
      #  proxy_pass http://portainer:9000/;
      #}

      #location /prometheus/ {
      #  proxy_pass http://prometheus:9090/;
      #}

      #location /grafana/ {
      #  proxy_pass http://grafana:3000/;
      #}

   }
}
```

The home page can be edited by:

```
sudo nano /var/lib/docker/volumes/nginx_data/_data/index.html
```

You can put there something like this:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to Docker Playground!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to Docker Playground!</h1>
<p>This is your personal Docker playground with various management, monitor and helping tools..</p>

<h2>Tools you can use:</h2>

<ul>
  <li><a href="http://localhost:5000/portainer/">Portainer</a> - for managing Docker.</li>
  <li><a href="http://localhost:5000/prometheus/">Prometheus</a> - the god of monitoring.</li>
  <li><a href="http://localhost:5000/grafana/">Grafana</a> - to see various monitoring dashboards.</li>
</ul>

<p><em>Thank you for using Docker Playground.</em></p>
</body>
<html>
```

NOTE: The `localhost:5000` is the VM/Playground and the external port of the nginx running on it.
