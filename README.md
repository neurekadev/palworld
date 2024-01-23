# Palworld

A docker container for easy deployment and configuration of a [Palworld](https://store.steampowered.com/app/1623730/Palworld/) dedicated server.

## Server Setup

### Login as Steam user

> [!IMPORTANT]  
> Palworld dedicated servers can only be setup by a user that has already purchased Palworld.

```docker
docker run --rm -it \
    -v "palworld_userdata:/home/steam/Steam" \
    cm2network/steamcmd ./steamcmd.sh +login <username> +quit
```

### Start the Palworld server

#### Docker Run (Easy)
```docker
docker run \
    --name palworld \
    --net host \
    --user 1000:1000 \
    --env STEAM_USERNAME=<username> \
    --publish 8211:8211 \
    --publish 15636:15636 \
    --publish 15637:15637 \
    --volume palworld_userdata:/home/steam/Steam \
    --volume palworld_data:/home/steam/Steam/steamapps/common/PalServer \
    ghcr.io/douglasparker/palworld:latest
```

#### Docker Compose (Recommended)

```docker
services:
  palworld:
    image: ghcr.io/douglasparker/palworld:latest
    container_name: palworld
    network_mode: host
    user: 1000:1000
    environment:
      - STEAM_USERNAME=<username>
    ports:
      - 8211:8211
      - 15636:15636
      - 15637:15637
    volumes:
      - userdata:/home/steam/Steam
      - data:/home/steam/Steam/steamapps/common/PalServer
    restart: unless-stopped

volumes:
  data:
    name: palworld_data
  userdata:
    name: palworld_userdata
```