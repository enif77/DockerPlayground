# Docker Compose

Docker compose is optional, but allows you to build images/containers using a script (aka Dockerfile).

Download and install using the description here:

https://docs.docker.com/compose/install/

## Download and install

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

NOTE: The URL contains a version. Check the above link to get the latest version.

Make it executable:

```
sudo chmod +x /usr/local/bin/docker-compose
```

You can check, if it is working by running:

```
docker-compose --version
```
