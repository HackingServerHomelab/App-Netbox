# App-Netbox

## First Time Prerequisites

1. `rm ./Data/postgres/.gitkeep`
2. Run [Traefik](https://github.com/HackingServerHomelab/App-Traefik)

## Running the Containers

1. Update the following volumes in [docker-compose.yml](./Docker/docker-compose.yml)
    * ` ../Data/netbox:/config`
    * `../Data/postgres:/var/lib/postgresql/data`
1. Update the following environment variables in [docker-compose.yml](./Docker/docker-compose.yml)
    * `SUPERUSER_EMAIL=admin`
    * `SUPERUSER_PASSWORD=changeme`
    * `DB_PASSWORD=changeme`
    * `POSTGRES_PASSWORD=changeme`
2. Update the Traefik host label in [docker-compose.yml](./Docker/docker-compose.yml)
    * ``"traefik.http.routers.netbox.rule=Host(`localhost`)"``
3. Run `docker-compose -f ./Docker/docker-compose.yml up -d`

## First Time Setup

1. Visit <https://your-ip>
2. Log in with the credentials you configured in the `SUPERUSER_EMAIL` and `SUPERUSER_PASSWORD` variables
