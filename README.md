# Dockerized Maniaplanet Shootmania Elite Dedicated Server
A  Docker image for running ManiaPlanet (Shootmania) with the Elite title script (game mode).
## Requirements
A machine running with Docker engine.

docker
docker-compose

## Usage

### Directly from CLI

We advice you to use the docker-compose method. CLI is not documented.

### With docker-compose

##### **Env file:**
[Create mp_dedicated_user/mp_dedicated_pass](https://www.maniaplanet.com/account/dedicated-servers) if you don't have it already.

Make a copy of `dedicated_vars.default.env` and rename to `dedicated_vars.env`, edit the following to your liking:
```
LOGIN=mp_dedicated_user
PASSWORD=mp_dedicated_pass
TITLE=SMStormElite@nadeolabs
TITLE_PACK_URL=https://maniaplanet.com/ingame/public/titles/download/SMStormElite@nadeolabs.Title.Pack.gbx
TITLE_PACK_FILE=SMStormElite@nadeolabs.Title.Pack.gbx
MATCH_SETTINGS=MatchSettings/default.txt
```

##### **Docker-compose entry:**
docker-compose.yml
```yml
  dedicated:
    image: pyplanet/maniaplanet-dedicated
    restart: always
    env_file: ./dedicated_vars.env
    volumes:
      - ./UserData:/dedicated/UserData
      - ./Logs:/dedicated/Logs
    expose:
      - "2350"
      - "2350/udp"
      - "3450"
      - "3450/udp"
      - "5000"
    ports:
      - 5000:5000
      - 2350:2350
      - "2350/udp"
      - "3450"
      - "3450/udp"
```

**Authorization**
Make your DEFAULT changes to the authorization_levels part in `config.default.xml` (**change** the SuperAdmin, Admin credentials etc). 
You should NOT have to set the server name etc here.
```xml
    <authorization_levels>
        <level>
            <name>SuperAdmin</name>
            <password>SuperAdmin</password>
        </level>
        <level>
            <name>Admin</name>
            <password>Admin</password>
        </level>
        <level>
            <name>User</name>
            <password>User</password>
        </level>
    </authorization_levels>
```

**Maps**
Add to or Replace the maps in "EliteMaps" (they will be copied when you build the image)

### Port Forwarding 
These ports needs to be open on the host
- 2350 tcp & udp (Game, General)
- 3450 tcp & udp (P2P)
- 5000 tcp
The XML-RPC port (5000) SHOULD NOT BE OPENED TO PUBLIC unless you change the <authorization_levels> logins and passwords in the dedicated configuration.

### TESTING
> docker-compose build

then

> docker-compose run


# Useful Links
Maniaplanet scripts and resources: https://github.com/maniaplanet/game-modes

Title packs, Network configuration: https://doc.maniaplanet.com/dedicated-server/getting-started

Original repo by PyPlanet (TrackMania): https://github.com/PyPlanet/maniaplanet-docker

You might be interested in this too: https://github.com/zagorim/docker-speedball