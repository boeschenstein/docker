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

build image (use slash as namespace separator)

>Create hello World node app: <https://medium.com/@adnanrahic/hello-world-app-with-node-js-and-express-c1eb7cfa8a30>

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

Delete all unused images

```dos
docker image prune
```

## Container (instance of image)

Start container

```dos
docker run -p 8000:3000 <image-name>
example:
docker run -p 8000:3000 bopa/node
```

App will run on Port 8000 (3000 is internal port)

Stop all containers (bash)

```bash
docker stop $(docker ps -a -q)
```

BEWARE: To delete all containers including its volumes use (in bash)

```bash
docker rm -vf $(docker ps -a -q)
```

Remove container by container-name

```bash
docker rm $(docker stop $(docker ps -a -q --filter name=containerName1 --format="{{.ID}}"))
```

Delete all unused containers

```dos
docker container prune
```

## Volume

Volumes or data volumes is a way for us to create a place in the host machine where we can write files so they are persisted.

Create

```dos
docker volume create <name of volume>
```

List

```dos
docker volume ls
```

Inspect

```dos
docker inspect <name of volume or container>
```

Find IP address:

```dos
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" <containername>
```

Delete

```dos
docker volume rm <name of volume>
```

Delete all unused volumes

```dos
docker volume prune
```

## Connect to running docker container

```dos
docker exec -it <containername> bash
```

## System

BEWARE: delete everything

```bash
docker system prune -af
```

## Windows vs. Linux Containers

- Linux does not support Windows Authentication

### Difference in csproj

```xml
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <DockerDefaultTargetOS>Windows</DockerDefaultTargetOS>
  </PropertyGroup>
```

```xml
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <DockerDefaultTargetOS>Linux</DockerDefaultTargetOS>
  </PropertyGroup>
```

## Tutorials

### Build inside container vs. build outside container

Compare: build inside vs. build outside container: <https://docs.docker.com/samples/dotnetcore/>

### Dockerize an ASP.NET Core application (build within container)

<https://docs.microsoft.com/en-us/visualstudio/containers/container-build?view=vs-2019>

Create new WebApi project in Visual Studio, option [x] Enable Docker checked

```docker
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["WebApplication1/WebApplication1.csproj", "WebApplication1/"]
RUN dotnet restore "WebApplication1/WebApplication1.csproj"
COPY . .
WORKDIR "/src/WebApplication1"
RUN dotnet build "WebApplication1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication1.dll"]
```

Make sure, app is running in 'Docker' (not Kestrel, not IISExpress) and press F5.\
While app is running in Docker now, you are even able to debug the app. (set a breakpoint to confirm)

### Windows Container

Go to folder of csproj and build the container

`docker build -t bopa/aspnetcore_windows -f Dockerfile ..`

Start the container

`docker run -p 8001:80 bopa/aspnetcore_windows`

Test WebApi: <http://localhost:8001/weatherforecast>

### Linux Container

Go to folder of csproj and build the container

`docker build -t bopa/aspnetcore_linux -f Dockerfile ..`

Start the container

`docker run -p 8002:80 bopa/aspnetcore_linux`

Test WebApi: <http://localhost:8002/weatherforecast>

## Demo From Docker

Details/Source: <https://github.com/docker/getting-started>

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

Run detached container with port mapping

```cmd
docker run -d -p 8888:80 --name container-name> <image-name>
```

Pull/Push

```cmd
docker pull <dockerhubuser>/<imagename>
docker push <dockerhubuser>/<imagename>
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

## Dotnet Core Examples

<https://hub.docker.com/_/microsoft-dotnet-core>

## Information

### Kubernetes is dropping Docker support - What does it mean for YOU?

- Blog: <https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/>
- Video: Nothing for Developers and DevOps, according <https://youtu.be/7KUdmFyefSA?t=289>
