# Netbox

Netbox has a docker project located [here](https://github.com/netbox-community/netbox-docker).
This directory includes the docker compose file from that project, with a few minor tweaks to work
on my own macvlan network.


## First Time Prerequisites

1. `git submodule init && git submodule update`
2. Install [mkcert](https://github.com/FiloSottile/mkcert#installation)
    1. `apt-get install -y libnss3-tools`
    2. `wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.1/mkcert-v1.4.1-linux-amd64`
    3. `chmod +x mkcert-v1.4.1-linux-amd64`
    4. `sudo mv mkcert-v1.4.1-linux-amd64 /usr/local/bin`

## Running the Containers

1. Update the following environment variables in [docker-compose.override.yml](./Docker/docker-compose.override.yml)
    * `AUTH_LDAP_SERVER_URI: "ldaps://domain.com"`
    * `AUTH_LDAP_BIND_DN: "CN=Netbox,OU=BindUsers,OU=Company,DC=domain,dc=com"`
    * `AUTH_LDAP_BIND_PASSWORD: "BindPassword"`
    * `AUTH_LDAP_USER_SEARCH_BASEDN: "OU=Users,OU=Company,DC=domain,dc=com"`
    * `AUTH_LDAP_GROUP_SEARCH_BASEDN: "OU=Groups,OU=Company,DC=domain,dc=com"`
    * `AUTH_LDAP_REQUIRE_GROUP_DN: "CN=Netbox-User,OU=SoftwareGroups,OU=SubGroups,OU=MyCompany,DC=domain,dc=com"`
    * `AUTH_LDAP_IS_ADMIN_DN: "CN=Network Admins,OU=Groups,OU=MyCompany,DC=domain,dc=com"`
    * `AUTH_LDAP_IS_SUPERUSER_DN: "CN=Domain Admins,CN=Users,DC=domain,dc=com"`
    * `LDAP_IGNORE_CERT_ERRORS: "true"`
2. `cp ./Docker/docker-compose.override.yml ./netbox-docker/docker-compose.override.yml`
3. `mkcert -install && mkcert localhost 127.0.0.1 ::1 && cat localhost+2.pem localhost+2-key.pem >> ./netbox-docker/localhost+2-full.pem`
4. Modify the following settings in [./netbox-docker/env/netbox.env](https://github.com/netbox-community/netbox-docker/blob/release/env/netbox.env)
    * `SUPERUSER_PASSWORD`
    * `SUPERUSER_API_TOKEN`
    * `DB_PASSWORD`
    * `SECRET_KEY`
    * `EMAIL_PASSWORD`
    * `NAPALM_PASSWORD`
    * `REDIS_PASSWORD`
    * `REDIS_CACHE_PASSWORD`
    * `AUTH_LDAP_BIND_PASSWORD`
    * `EMAIL_SERVER`
    * `EMAIL_PORT`
    * `EMAIL_USERNAME`
    * `EMAIL_PASSWORD`
    * `EMAIL_FROM`
    * `EMAIL_USE_TLS`
    * `EMAIL_SSL_CERTFILE`
    * `EMAIL_SSL_KEYFILE`
    * `SUPERUSER_EMAIL`
5. Modify the following settings in [./netbox-docker/env/redis.env](https://github.com/netbox-community/netbox-docker/blob/release/env/redis.env)
    * `REDIS_PASSWORD`
6. Modify the following settings in [./netbox-docker/env/redis-cache.env](https://github.com/netbox-community/netbox-docker/blob/release/env/redis-cache.env)
    * `REDIS_PASSWORD`
7. `cd ./netbox-docker && docker-compose up -d`


## First Time Setup

1. Visit <http://your-ip>
2. Log in with the default admin credentials: `admin:admin`, or the credentials you set via environment variables
