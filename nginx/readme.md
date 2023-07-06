# Nginx with docker

## Flow

### 1. Put the .conf file in the nginx/conf folder with port 80

Put the .conf file in the nginx/conf folder with only server port 80 to generate certificate

example: shawnsavour.conf

If certificate is not generated, use following command to generate certificate

```bash
docker compose run certbot
```

### 2. Update the server 443 to the .conf file

Update the server 443 to the .conf file, after update restart compose

```bash
docker compose restart
```

### 3. All set

