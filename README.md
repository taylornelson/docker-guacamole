## 1 shot
``` shell
docker run -p 8080:8080 -v /tmp:/config oznu/guacamole
```

extensions are really useful for more security, ldap auth idk

## Usage

```shell
docker run \
  -p 8080:8080 \
  -v </path/to/config>:/config \
  oznu/guacamole
```

## Raspberry Pi / ARMv6

This image will also allow you to run [Apache Guacamole](https://guacamole.apache.org/) on a Raspberry Pi or other Docker-enabled ARMv5/6/7/8 devices by using the `armhf` tag.

```shell
docker run \
  -p 8080:8080 \
  -v </path/to/config>:/config \
  oznu/guacamole:armhf
```

## Parameters

The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side.

* `-p 8080:8080` - Binds the service to port 8080 on the Docker host, **required**
* `-v /config` - The config and database location, **required**
* `-e EXTENSIONS` - See below for details.

## Enabling Extensions

Extensions can be enabled using the `-e EXTENSIONS` variable. Multiple extensions can be enabled using a comma separated list without spaces.

For example:

```shell
docker run \
  -p 8080:8080 \
  -v </path/to/config>:/config \
  -e "EXTENSIONS=auth-ldap,auth-duo"
  oznu/guacamole
```

Currently the available extensions are:

* auth-ldap - [LDAP Authentication](https://guacamole.apache.org/doc/gug/ldap-auth.html)
* auth-duo - [Duo two-factor authentication](https://guacamole.apache.org/doc/gug/duo-auth.html)
* auth-header - [HTTP header authentication](https://guacamole.apache.org/doc/gug/header-auth.html)
* auth-cas - [CAS Authentication](https://guacamole.apache.org/doc/gug/cas-auth.html)
* auth-openid - [OpenID Connect authentication](https://guacamole.apache.org/doc/gug/openid-auth.html)
* auth-totp - [TOTP two-factor authentication](https://guacamole.apache.org/doc/gug/totp-auth.html)

You should only enable the extensions you require, if an extensions is not configured correctly in the `guacamole.properties` file it may prevent the system from loading. See the [official documentation](https://guacamole.apache.org/doc/gug/) for more details.

## Default User

The default username is `guacadmin` with password `guacadmin`.

## Windows-based Docker Hosts

Mapped volumes behave differently when running Docker for Windows and you may encounter some issues with PostgreSQL file system permissions. To avoid these issues, and still retain your config between container upgrades and recreation, you can use the local volume driver, as shown in the `docker-compose.yml` example below. When using this setup be careful to gracefully stop the container or data may be lost.

```yml
version: "2"
services:
  guacamole:
    image: oznu/guacamole
    container_name: guacamole
    volumes:
      - postgres:/config
    ports:
      - 8080:8080
volumes:
  postgres:
    driver: local
```
