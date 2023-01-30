# docker-compose-media-center

This respository contains the Docker compose files for standing up the entire
stararr stack along with a few other tools and services. The following tools and
services are included (links are to the GitHub for the Docker container):

## `docker-compose-base.yml`

- [Syncthing]()
- [Tailscale]()
- [Watchtower]()

## `docker-compose-downloaders.yml`

- [Deluge](): Deluge with OpenVPN and Wireguard support and killswitch.
  **NOTE:** This is also the *base* container for the VPN connection and
  killswitch which all other containers in this Portainer stack are routed
  through.
- [NZBGet](): NZBGet for Usenet downloads.
- [SABnzbd]() SABnzbd for Usenet downloads.
- [Transmission](): Transmission for torrent downloads.
- [Unpackerr](): Unpackerr for any unpacking in post-processing after download.

## `docker-compose-stararr.yml`

- [Bazarr](): for adding Subtitles to media found in Radarr and Sonarr.
- [Lidarr](): for Music.
- [Prowlarr]():
- [Radarr](): for Movies.
- [Readarr](): for eBooks and Audiobooks.
- [Sonarr](): for TV series and shows.

## `docker-compose-plex.yml`

- [Plex](): Plex nedia center.
- [Tautulli](): Tautulli for Plex statistics and monitoring.
- [Overseerr](): Overseer for browsing and discovering of new media.
- [Notifiarr](): Notifiarr for Discord and Webhooks.

# Setup and installation

1. Install Docker

2. Create the `media` network with:

```docker-compose
  docker network create \
    --driver=bridge \
    --gateway=172.16.0.1 \
    --ip-range=172.16.0.0/24 \
    --subnet=172.16.0.0/16 \
    -o "com.docker.network.bridge.enable_icc"="true" \
    -o "com.docker.network.bridge.enable_ip_masquerade"="true" \
    -o "com.docker.network.bridge.host_binding_ipv4"="0.0.0.0" \
    -o "com.docker.network.bridge.name"="media" \
    -o "com.docker.network.driver.mtu"="1500" \
    media
```

3. Edit and move the file `environment_variables.env.example` saving it to the file to
`.env` substituting with values you have after starting and setting up many of
the services and applications.

4. Run `docker-compose -f [docker-compose-XXXXXX.yml] up`: in the following
order:

- `docker-compose --file docker-compose-base.yaml --env-file .env up`
- `docker-compose --file docker-compose-downloaders.yml --env-file .env up`
- `docker-compose --file docker-compose-stararr.yml --env-file .env up`
- `docker-compose --file docker-compose-plex.yml --env-file .env up`
