# Portainer

Portainer is for managing Docker.

- https://www.portainer.io/
- https://documentation.portainer.io/v2.0/deploy/ceinstalldocker/
- https://documentation.portainer.io/v2.0/deploy/initial/

Create a named volume for storing Portainer data:

```
docker volume create portainer_data
```

Download and run Portainer:

```
docker run \
  -d \
  -p 8000:8000 \
  -p 9000:9000 \
  --name=portainer \
  --restart=always \
  --net docker-net \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce
```

Add in Virtual Box settings port mapping for port 9000, so you can access Portainer from your host machine. You can access Portainer UI by opening URL:

```
http://localhost:9000/

```

First run of Portainer will ask for setting a password to the user `admin`. Set it to, for example, `Passw0rd1`. 

Manage the Docker in VM using the "Manage the local Docker environment" choice.
