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
Grafana requires to set a password for the default `adimin` user. I am using the `Passw0rd1` password.

Grafana configuration can be edited by:

```
sudo nano /var/lib/docker/volumes/grafana_config/_data/grafana.ini
```
