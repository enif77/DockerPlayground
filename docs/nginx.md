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

nginx should contain module for publishing metrics. You can test it by running following command inside of its container:

```
nginx -V 2>&1 | grep -o with-http_stub_status_module
```
The configuration file can be edited by:

```
sudo nano /var/lib/docker/volumes/nginx_config/_data/nginx.conf
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

NOTE: The localhost:5000 is the VM/Playground and the external port of the nginx running on it.
