Most config that you need to adapt is in file
`.env` = `env` (symlink)

# Main web server

The main webserver is caddy, which acts as reverse proxy for the other web
servers. It fully automatically fetches and renews the SSL certificates from
LetsEncrypt and stores them in `caddy/data/`.
The config is in `caddy/caddyfile`, which contains the references to the
other servers.
Caddy runs in a docker container and is written in Go, see <https://caddyserver.com>.

# Web Frontend

Web-frontend is the user-facing web app for the video conference, both admin
and conference UI.
Its docker runs an nginx, but that is not the main web server.

# Controller

Controller is managing the video conf rooms.
It is a REST http API, written in Rust.
Its config file is in `config/controller.toml`. The passwords, databases etc.
It needs to match what is in `.env`, `docker-compose.yaml` and the other configs.

# Keycloak

Keycloak is user auth using OAuth2 and OpenID Connect.
It automatically imports the settings from the `export.json` file
into `data/kc_data/`. Or does controller set keycloak up automatically?

# MinIO

MinIO is like S3 object storage.
The bucket is created either with `md` or
by simply creating the directory on disk in `data/minio/<bucketname>/`
The bucket name is also in `config/controller.toml`
The admin password is in env.
