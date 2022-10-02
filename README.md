# XMage Server Docker Images
This Project automates the creation of Docker Images for every new versions of [XMage](https://github.com/magefree/mage) Server. 

**There has been no official release of XMage since over a year. Most People are playing on the beta version now.**

You can find Docker Images for the Beta Server here: https://github.com/mage-docker/xmage-beta-docker

The latest beta client can be donloaded at http://xmage.today

## Usage

All Images are taged with the version number. So you can run older versions by including the tag when creating the container. You can see all available tags under tags.


    docker run --rm -it \
        -p 17171:17171 \
        -p 17179:17179 \
        --add-host example.com:0.0.0.0 \
        -e "XMAGE_DOCKER_SERVER_ADDRESS=example.com" \
        goesta/xmage-server


XMage needs to know the domain name the server is running on. The `--add-host` option adds an entry to the containers `/etc/hosts` file for this domain. 
Using the `XMAGE_*` environment variables you can modify the `config.xml` file.
You should always set `XMAGE_DOCKER_SERVER_ADDRESS` to the same value as `--add-host`.

If you dont have a Domain you can use a service like http://nip.io.

If you like to preserve the database during updates and restarts you can mount a volume at /xmage/db


## Example Docker Compose file

    version: '2'
    services:
    mage:
        image: goesta/xmage-server
        ports:
         - "17171:17171"
         - "17179:17179"
        extra_hosts:
         - "example.com:0.0.0.0"
        environment:
         - XMAGE_DOCKER_SERVER_ADDRESS=example.com
         - XMAGE_DOCKER_SERVER_NAME=xmage-server
         - XMAGE_DOCKER_MAX_SECONDS_IDLE=6000
         - XMAGE_DOCKER_AUTHENTICATION_ACTIVATED=false
        volumes:
         - xmage-db:/xmage/db
         - xmage-saved:/xmage/saved
    volumes:
        xmage-db:
            driver: local
        xmage-saved:
            driver: local

## Links

[Tutorial - Running XMage on DigitalOcean](https://github.com/goesta/docker-xmage-alpine/wiki/DigitalOcean-Tutorial)
