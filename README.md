# Docker

## Terminology

Image: build of an app
Container: instance of a build

### Disable VirtualBox, activate Hyper-V

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
```

Hyper-V for Docker needed
Hyper-V Manager

```cmd
bcdedit /set hypervisorlaunchtype off (deactivate Hyper-V)
bcdedit /set hypervisorlaunchtype auto (reactivate Hyper-V)
```

reboot
press info-button to activate hyper-v when docker starts

### Hyper-V

needs Hyper-V (needs to disable VirtualBox)

check Hyper-V Manager

## Dockerfile

dockerfile = Recipe to create a docker image

simple Dockerfile

```dockerfile
FROM node:latest

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

ENTRYPOINT ["node", "app.js"]
```

## Images

build

```dos
docker build -t <image-name> .
example:
docker build -t bopa/node:latest .
```

show all images

```dos
docker images
```

BEWARE: delete image

```dos
docker image rm <name>
or
docker rmi <container>
```

BEWARE: Remember, you should remove all the containers before removing all the images from which those containers were created.

BEWARE: To delete all the images, run this:

```bash
docker rmi -f $(docker images -a -q)
```

## Container (instance of image)

start container

```dos
docker run -p 8000:3000 <image-name>
example:
docker run -p 8000:3000 bopa/node
```

stop all containers (bash)

```bash
docker stop $(docker ps -a -q)
```

BEWARE: To delete all containers including its volumes use (in bash)

```bash
docker rm -vf $(docker ps -a -q)
```

remove container by container-name

```bash
docker rm $(docker stop $(docker ps -a -q --filter name=containerName1 --format="{{.ID}}"))
```

## Volume

Volumes or data volumes is a way for us to create a place in the host machine where we can write files so they are persisted.

create

```dos
docker volume create <name of volume>
```

list

```dos
docker volume ls
```

inspect

```dos
docker inspect <name of volume>
```

delete

```dos
docker volume rm <name of volume>
```

delete all unused volumes

```dos
docker volume prune
```

## System

BEWARE: delete everything

```bash
docker system prune -af
```

## Tutorials

### Dockerize an ASP.NET Core application

https://docs.docker.com/engine/examples/dotnetcore/

### Demo From Docker

Details/Source: https://github.com/docker/getting-started

Clone:

```cmd
git clone https://github.com/docker/getting-started.git
```

Build:

```cmd
cd getting-started
docker build -t docker101tutorial .
```

Run:

```cmd
docker run -d -p 80:80 \ --name docker-tutorial docker101tutorial
```

Share: (optional)

```cmd
docker tag docker101tutorial {userName}/docker101tutorial
docker push {userName}/docker101tutorial
```

Start:

Press the play button in the "Docker Desktop" app.

> If you are running a webserver on localhost (like IIS), you have to stop it first. Otherwise the container won't start.

`http://localhost/`

Continue this Tutorial in the Browser now.

### Dotnet Core Examples

https://hub.docker.com/_/microsoft-dotnet-core

### Kubernetes is dropping Docker support - What does it mean for YOU?

Nothing for Developers and DevOps, according https://youtu.be/7KUdmFyefSA?t=289
