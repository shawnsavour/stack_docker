# Basic docs for Traefik

## Configure Traefik

```yaml
traefik:
    image: "traefik:v2.10"
    container_name: "traefik"
    command:
        #- "--log.level=DEBUG"
        - "--api.insecure=true"
        - "--providers.docker=true"
        - "--providers.docker.exposedbydefault=false"
        # redirect http to https
        - "--entrypoints.web.address=:80"
        - "--entrypoints.web.http.redirections.entryPoint.to=websecure"
        - "--entrypoints.web.http.redirections.entryPoint.scheme=https"
        # https
        - "--entrypoints.websecure.address=:443"
        - "--certificatesresolvers.myresolver.acme.tlschallenge=true"
        #- "--certificatesresolvers.myresolver.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory"
        - "--certificatesresolvers.myresolver.acme.email=youremail@gmail.com"
        - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
    ports:
        - "80:80"
        - "443:443"
        - "8080:8080"
    volumes:
        - "./letsencrypt:/letsencrypt"
        - "/var/run/docker.sock:/var/run/docker.sock:ro"
```

## Configure Docker

### Using Traefik for docker
```yaml
whoami:
    image: "traefik/whoami"
    container_name: "simple-service"
    labels:
      - "traefik.enable=true"
```

### Routing domain to docker
```yaml
labels:
- "traefik.enable=true"
# router Host and Path
- "traefik.http.routers.SERVICE_NAME.rule=Host(`example.shawnsavour.com`) && PathPrefix(`/api`)"
- "traefik.http.services.SERVICE_NAME.loadbalancer.server.port=80"
```

### Using middleware
```yaml
labels:
- "traefik.http.routers.SERVICE_NAME.middlewares=MIDDLEWARE_NAME"
- "traefik.http.middlewares.MIDDLEWARE_NAME.stripprefix.prefixes=/api"
```

