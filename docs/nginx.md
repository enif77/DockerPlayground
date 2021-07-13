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
