# Docker

Description of how to install and setup Docker. 

## Interesting links

- https://docs.docker.com/
- https://docs.docker.com/engine/install/ubuntu/
- https://docs.docker.com/engine/install/linux-postinstall/
- https://docs.docker.com/config/daemon/prometheus/
- https://docs.docker.com/engine/reference/commandline/run/
- https://www.tutorialworks.com/container-networking/

## Installation

Install using Docker tutorial for installing from the from Docker repository.

Add the actual user (`kid`) to the `docker` user group (log out+login activates this add-to-group):

```
sudo usermod -aG docker $USER
```

Modify/create the Docker daemon configuration file:

https://docs.docker.com/config/containers/logging/configure/#configure-the-default-logging-driver
https://docs.docker.com/config/containers/logging/json-file/

```
sudo nano /etc/docker/daemon.json                           
```

Insert this to it and restart the Docker service (or the whole VM):

```
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3" 
  },
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```

Use different logging setup, when needed.

## Docker network

All docker containers, that should see each other via name, should be inside the same network. The following command will create a network, which we will be using in all later examples.

```
docker network create docker-net
```

## Additional info

All docker named volumes are stored in directory:

```
/var/lib/docker/volumes
```
