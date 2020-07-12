# Docker

## Demo

Clone:

```cmd
git clone https://github.com/docker/getting-started.git
```

Buil:

```cmd
cd getting-started
docker build -t docker101tutorial .
```

Run:

```cmd
docker run -d -p 80:80 \ --name docker-tutorial docker101tutorial
```

Share:


```cmd
docker tag docker101tutorial {userName}/docker101tutorial
docker push {userName}/docker101tutorial
```
