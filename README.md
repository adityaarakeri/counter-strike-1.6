# Counter-Strike 1.6 Server Docker Image

This image is based on `debian:9` and the game server is set up via steamcmd.
It aims to provide a simple method to set up a basic and also customizable Counter-Strike 1.6 server via Docker.


## Quick start

The fastest way to set this up is to pull the image and start it via `docker run`.

``` bash
docker pull secret105v/counterstrike
```

``` bash
docker run --name counter-strike_server -p 27015:27015/udp -p 27015:27015 secret105v/counterstrike
```

However it's recommend to run the server via `docker-compose`. You can find an example docker-compose.yml below.

## Available environment variables

| Variable   | Value    |
| ---------- | -------- |
| PORT       | 27015    |
| MAP        | de_dust2 |
| MAXPLAYERS | 16       |
| SV_LAN     | 0        |

## Custom config files

You can add you own `server.cfg`, `banned.cfg`, `listip.cfg` and `mapcycle.txt` by linking them as volumes into the image.

``` bash
-v /path/to/your/server.cfg:/hlds/cstrike/server.cfg
```

The complete command looks like this:

``` bash
docker run --name counter-strike_server -p 27015:27015/udp -p 27015:27015 -v /home/server.cfg:/hlds/cstrike/server.cfg secret105v/counterstrike
```

Keep in mind the server.cfg file can override the settings from your environment variables:  
`MAP`, `MAXPLAYERS` and `SV_LAN`

### Example server.cfg

```
// Use this file to configure your DEDICATED server.
// This config file is executed on server start.

// disable autoaim
sv_aim 0

// disable clients' ability to pause the server
pausable 0

// default server name. Change to "Bob's Server", etc.
hostname "Counter-Strike 1.6 Server"

// RCON password
rcon_password "password"

// default map
map de_dust2

// maximum client movement speed
sv_maxspeed 320

// 20 minute timelimit
mp_timelimit 20

// disable cheats
sv_cheats 0

// load ban files
exec listip.cfg
exec banned.cfg
```

## Docker Compose

Create a `docker-compose.yml` file and start the server via `docker-compose up -d`.

### Example docker-compose.yml

``` yml
version: '3'

services:

  hlds:
    container_name: counterstrike
    image: secret105v/counterstrike
    restart: always
    environment:
      - PORT=27015
      - MAP=de_dust2
      - MAXPLAYERS=16
      - SV_LAN=0
    ports:
      - 27015:27015/udp
      - 27015:27015
    volumes:
      - /path/to/your/banned.cfg:/hlds/cstrike/banned.cfg
      - /path/to/your/listip.cfg:/hlds/cstrike/listip.cfg
      - /path/to/your/server.cfg:/hlds/cstrike/server.cfg
      - /path/to/your/mapcycle.txt:/hlds/cstrike/mapcycle.txt
```
