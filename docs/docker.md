# Docker

Description of how to install and setup [Docker](https://www.docker.com). 

## Interesting links

- https://docs.docker.com/
- https://docs.docker.com/engine/install/ubuntu/
- https://docs.docker.com/engine/install/linux-postinstall/
- https://docs.docker.com/config/daemon/prometheus/
- https://docs.docker.com/engine/reference/commandline/run/
- https://docs.docker.com/storage/volumes/
- https://www.tutorialworks.com/container-networking/

## Installation

Install using [tutorial for installing from the Docker repository](https://docs.docker.com/engine/install/ubuntu/). Continue with the [linux-postinstall tutorial](https://docs.docker.com/engine/install/linux-postinstall/).

### Install using the repository

```
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Verify that Docker Engine is installed correctly by running the hello-world image.

```
sudo docker run hello-world
```

## Post install configuration

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
  "features" : {"buildkit" : true},
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```

This will change logging format to JSON with a maximal log file size and a limited amount of log files. It also enables [Prometheus](https://prometheus.io/) metrics endpoint, so the Docker can be monitored with Prometheus.

Use different logging setup, when needed. The Prometheus metrics endpoint is not necessary, when you are not planning to monitor running Docker containers with Prometheus.

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

## Saving and loading images

https://stackoverflow.com/questions/23935141/how-to-copy-docker-images-from-one-host-to-another-without-using-a-repository

```
docker images

docker save -o <path for generated tar file> <image name>
docker load -i <path to image tar file>
```

```
docker save -o c:/myfile.tar centos:16
```
