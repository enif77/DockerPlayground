# Grafana

Grafana is used to display metrics in user friendly way.

- https://grafana.com/docs/grafana/latest/
- https://grafana.com/docs/grafana/latest/administration/configure-docker/

Create a named volumes for storing Grafana data and configuration:

```
docker volume create grafana_config
docker volume create grafana_data
```

Download and run Grafana:

```
docker run \
  -d \
  -p 3000:3000 \
  --name=grafana \
  --restart=always \
  --net docker-net \
  -v grafana_config:/etc/grafana \
  -v grafana_data:/var/lib/grafana \
  grafana/grafana
```

Add in Virtual Box settings port mapping for port 3000, so you can access Grafana from your host machine. You can access Grafana by opening URL:

```
http://localhost:3000/
```
Grafana requires to set a password for the default `admin` user. I am using the `Passw0rd1` password.

Grafana configuration can be edited by:

```
sudo nano /var/lib/docker/volumes/grafana_config/_data/grafana.ini
```

Modifications necesarry for runnning Grafana behind nginx proxy:

```
# The public facing domain name used to access grafana from a browser
;domain = localhost
domain = grafana

# Redirect to correct domain if host header does not match domain
# Prevents DNS rebinding attacks
;enforce_domain = false

# The full public facing url you use in browser, used for redirects and emails
# If you use reverse proxy and sub path specify full url (with sub path)
;root_url = %(protocol)s://%(domain)s:%(http_port)s/
root_url = %(protocol)s://%(domain)s:%(http_port)s/grafana/

# Serve Grafana from subpath specified in `root_url` setting. By default it is set to `false` for compatibility reasons.;serve_from_sub_path = false
serve_from_sub_path = true
```

Grafana metrics for Prometheus are available at URL:

```
http://localhost:3000/metrics
```

Prometheus config:

```
  - job_name: 'grafana'
    static_configs:
    - targets: ['grafana:3000']
```
