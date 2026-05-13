# LEINELAB

We are currently maintaining our own fork due to https://github.com/goauthentik/authentik/pull/16646

Deploy using:

```
cd /opt/docker/authentik/leinelab-server-head
git pull
cd ..
docker compose build
docker compose down
docker compose up -d
```
