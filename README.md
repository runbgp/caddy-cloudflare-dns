[version-image]: https://img.shields.io/github/v/release/runbgp/caddy-cloudflare-dns?style=for-the-badge
[version-url]: https://github.com/runbgp/caddy-cloudflare-dns/releases

[gh-actions-image]: https://img.shields.io/github/actions/workflow/status/runbgp/caddy-cloudflare-dns/main.yml?style=for-the-badge
[gh-actions-url]: https://github.com/runbgp/caddy-cloudflare-dns/actions

[dockerhub-image]: https://img.shields.io/docker/pulls/runbgp/caddy-cloudflare-dns?label=DockerHub%20Pulls&style=for-the-badge
[dockerhub-url]: https://hub.docker.com/r/runbgp/caddy-cloudflare-dns

[![Latest Release][version-image]][version-url]
[![caddy on DockerHub][dockerhub-image]][dockerhub-url]
[![Docker Build][gh-actions-image]][gh-actions-url]

# caddy-cloudflare-dns

Please see the official [Caddy Docker Image](https://hub.docker.com/_/caddy) for deployment instructions.

Builds are available at the following Docker repositories:

* GitHub Container Registry: [ghcr.io/runbgp/caddy-cloudflare-dns](https://ghcr.io/runbgp/caddy-cloudflare-dns)
* Docker Hub: [docker.io/runbgp/caddy-cloudflare-dns](https://hub.docker.com/r/runbgp/caddy-cloudflare-dns)

### Getting started

1. Add your `CLOUDFLARE_EMAIL` and `CLOUDFLARE_API_TOKEN` as environment variables to your `docker-compose.yml` or `docker-run` command.

##### `docker-compose.yml`
```
services:
  caddy:
    image: ghcr.io/runbgp/caddy-cloudflare-dns:latest
    restart: unless-stopped
    container_name: caddy
    ports:
      - "80:80"
      - "443:443"
    networks:
      - caddy
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
    environment:
      - CLOUDFLARE_EMAIL=$CLOUDFLARE_EMAIL
      - CLOUDFLARE_API_TOKEN=$CLOUDFLARE_API_TOKEN
      - ACME_AGREE=true

networks:
  caddy:
    name: caddy
```

##### `docker run`
```
docker run -it --name caddy \
   -p 80:80 \
   -p 443:443 \
   -v caddy_data:/data \
   -v caddy_config:/config \
   -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
   -e CLOUDFLARE_EMAIL=$CLOUDFLARE_EMAIL \
   -e CLOUDFLARE_API_TOKEN=$CLOUDFLARE_API_TOKEN \
   -e ACME_AGREE=true \
   ghcr.io/runbgp/caddy-cloudflare-dns:latest
```
#### Cloudflare API
You can obtain your [Cloudflare API token](https://support.cloudflare.com/hc/en-us/articles/200167836-Managing-API-Tokens-and-Keys) via the Cloudflare Portal. To create an API token with minimal scope, the following steps are needed:

1. Log into the Cloudflare dashboard, navigate to account settings and create an API token.
2. Grant the following permissions:
   * Zone / Zone / Read
   * Zone / DNS / Edit
2. Add the following to your Caddyfile's [tls directive](https://caddyserver.com/docs/caddyfile/directives/tls#tls). 

```
tls {$CLOUDFLARE_EMAIL} { 
  dns cloudflare {$CLOUDFLARE_API_TOKEN}
}
```

#### Image tags
[See available tags here](https://hub.docker.com/r/runbgp/caddy-cloudflare-dns/tags).


To select a specific version of `caddy`, set your [Docker image tag](https://docs.docker.com/engine/reference/run/#imagetag) to the Caddy version you'd like to use, e.g. `runbgp/caddy-cloudflare-dns:2.9.1`
