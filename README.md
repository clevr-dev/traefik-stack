> This project is part of the [clevr-compose-stacks](...). [Learn more...](...)

# traefik-stack

traefik + influxdb + chronograf

## Configure

1. Edit the docker-compose environment file `.env`

2. Create the necessary DNS records (or modify the hosts file) to ensure proper DNS resolution from the domain name to the Traefik instance:

* `traefik.clevrdev.com` <- traefik dashboard
* `traefikviz.clevrdev.com`  <- chronograf ui

3. Create a Docker network based on what is defined for `PUBLIC_NETWORK` in `.env`; e.g., `traefik`:

```shell
docker network create traefik

# docker network create traefik -d overlay --attachable
```

### mkcert

If you're not relying on LetsEncrypt for certificates (e.g., for local development), use mkcert to generate the following files in the `certs` directory:

```
docker-compose -f docker-compose.mkcert.yml up
```

The command will produce a wildcard certificate for the domain `DNS_DOMAIN` specified in `.env`, along with the root CA public certificate that issued it, then exit:

```
certs/
├── cert-key.pem
├── cert.pem
└── rootCA.pem
```

Trust the `rootCA.pem` cert on your local computer / tablet / browser. Instructions for trusting a CA in Chrome are [here](https://support.securly.com/hc/en-us/articles/206081828-How-to-manually-install-the-Securly-SSL-certificate-in-Chrome).

## Build

Building the image locally is only required if changes to the image are made.

```bash
docker-compose -f image/docker-compose.build.yml build
```

Publish changes to Docker registry:

```bash
docker-compose -f image/docker-compose.build.yml push
```

## Start

configure for use with local certs:

```bash
docker-compose -f docker-compose.yml -f docker-compose.local.yml up
```

configure for use with LetsEncrypt:

```bash
docker-compose -f docker-compose.yml -f docker-compose.letsencrypt.yml up
```

## Use

Access the app in your browser according to variables set in the environment file (`.env`): `https://<APP_DNS_NAME>.<DNS_DOMAIN>`; e.g:

- `https://traefik.clevrdev.com`
- `https://traefikviz.clevrdev.com`

With this in place, other apps from the [clevr-compose-stacks] repo can be configured and deployed with ease.

## References

- https://containo.us/blog/traefik-2-0-docker-101-fc2893944b9d/
- https://github.com/containous/blog-posts/blob/master/2019_09_10-101_docker/docker-compose-09.yml

## Support

TODO: Links
