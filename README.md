# traefik-stack

traefik + jaeger + influxdb + chronograf

> Configuration derived from [this blog](https://containo.us/blog/traefik-2-0-docker-101-fc2893944b9d/); [this example](https://github.com/containous/blog-posts/blob/master/2019_09_10-101_docker/docker-compose-09.yml).

## setup

1. Create a network named `traefik`:

* if all containers will be running from same host:

```
docker network create traefik
```

* If you want Traefik to watch for containers on remote hosts in a Docker Swarm cluster, create an attacheable overlay network:

```
docker network create -d overlay --attachable traefik
```

2. Populate the `.env` file with desired values.

3. Customize `docker-compose.yml`:
   * Set unique basic auth credentials by setting the `traefik.http.middlewares.authtraefik.basicauth.users` label of the `traefik` service.
   * As desired, *uncomment* `traefik.http.routers.traefikapi_secure.tls.certresolver=letsencrypt` to request a LetsEncrypt certificate for each desired service.

4. Create the necessary DNS records *or* modify your hosts file to ensure proper DNS resolution:

* `traefik.mydomain.com` <- traefik dashboard
* `jaeger.mydomain.com`  <- jaeger ui
* `graf.mydomain.com`    <- chronograf
* `whoami.mydomain.com`  <- example app

For example:

```
127.0.0.1 traefik.mydomain.com
127.0.0.1 jaeger.mydomain.com
127.0.0.1 whoami.mydomain.com
127.0.0.1 graf.mydomain.com
```

5. Launch the stack with:

```
docker-compose up
```

or,

```
docker-compose up --scale whoami=3
```

6. Access service(s) from the browser; e.g., `https://whoami.mydomain.com`

> Note: if not using LetsEncrypt, Traefik will issue self-signed certs that may require you "accept risk" upon first logon.

