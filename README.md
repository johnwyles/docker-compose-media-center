# docker-compose-media-center

This respository contains the Docker compose files for standing up an entire
Stararr stack along with a few other tools and services best suited for an
at-home NAS.

## Docker Compose Files and the Applications and Services within

### `docker-compose-portainer.yml`

- [Portainer](https://hub.docker.com/r/portainer/portainer) Portainer for
  managing the remaining "Stacks" (docker-compose files).

### `docker-compose-base.yml`

- [Syncthing](https://github.com/linuxserver/docker-syncthing) Syncthing for
  syncing files across machines.
- [Tailscale](https://hub.docker.com/r/tailscale/tailscale) Tailscale for VPN to
  home network and for sharing services and files.
- [Watchtower](https://github.com/containrrr/watchtower) Watchtower for watching
  updates to docker images and pulling them down.

### `docker-compose-downloaders.yml`

- [Deluge](https://github.com/linuxserver/docker-deluge): Deluge for torrent
  downloads.
- [gluetun](https://github.com/qdm12/gluetun): **NOTE:** This is the
  *base* container for the VPN connection and killswitch which all other
  containers in this Portainer stack are routed.
- [NZBGet](https://github.com/linuxserver/docker-nzbget): NZBGet for Usenet
  downloads.
- [SABnzbd](https://github.com/linuxserver/docker-sabnzbd) SABnzbd for Usenet
  downloads.
- [Transmission-OpenVPN](https://github.com/haugene/docker-transmission-openvpn):
  Transmission with OpenVPN for torrent downloads.
- [Unpackerr](https://hub.docker.com/r/golift/unpackerr): Unpackerr for any
  unpacking in post-processing after download.

### `docker-compose-stararr.yml`

- [Bazarr](https://github.com/linuxserver/docker-bazarr): Bazarr for adding
  Subtitles to media found in Radarr and Sonarr.
- [Lidarr](https://github.com/linuxserver/docker-lidarr): Lidarr for Music.
- [Prowlarr](https://github.com/linuxserver/docker-prowlarr): Prowlarr for
  adding Indexers to Stararr services.
- [Radarr](https://github.com/linuxserver/docker-radarr): Radarr for Movies.
- [Readarr](https://github.com/linuxserver/docker-readarr): Readarr for eBooks
  and Audiobooks.
- [Sonarr](https://github.com/linuxserver/docker-sonarr): Sonarr for TV series
  and shows.

### `docker-compose-plex.yml`

- [Plex](https://github.com/linuxserver/docker-plex): Plex nedia center for
  viewing and playing all of your media.
- [Tautulli](https://github.com/linuxserver/docker-tautulli): Tautulli for Plex
  statistics and monitoring.
- [Overseerr](https://github.com/linuxserver/docker-overseerr): Overseer for
  browsing and discovering of new media.
- [Notifiarr](https://hub.docker.com/r/golift/notifiarr): Notifiarr for Discord
  and Webhooks.

## Installation and Setup

1. Install Docker

2. Create a `media` network with:

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

3. Edit and move the file `environment_variables.env.example` saving it to the
file to `.env` substituting with values you have after starting and setting up
many of the services and applications. **IMPORTANT NOTE:** Complete these
sections:

- [Directory Structure](#directory-structure),
- [Environment Variables](#environment-variables)
- [Transmission OpenVPN Setup](#transmission-openvpn-setup)

You **must** complete these steps in this README *before* moving on to the next.

4. Run these `docker compose` commands order (**Note:** there are no
  `depends_on`):

- First things first so we get the Portainer instance going:
  - `docker compose --file docker-compose-portainer.yaml --env-file .env up --detach`

- And now for the rest:
  - `docker compose --file docker-compose-base.yaml --env-file .env up --detach`
  - `docker compose --file docker-compose-downloaders.yml --env-file .env up --detach`
  - `docker compose --file docker-compose-stararr.yml --env-file .env up --detach`
  - `docker compose --file docker-compose-plex.yml --env-file .env up --detach`

## Directory Structure

In order to start you need to create several directories to house the
configurations for each service and where the Docker container will persist that
configuration locally between reboots, restarts, etc. This maps directly to the
`CONFIG_BASE_DIR` directory in the `.env` file we will get to later.

```shell
export CONFIG_BASE_DIR=/volume1/data
mkdir -p ${CONFIG_BASE_DIR}
```

This sets the `CONFIG_BASE_DIR` directory that will store all of the subsequent
directories for their Docker configurations. To create each of those directories
run the following:

```shell
mkdir -p ${CONFIG_BASE_DIR}/bazarr
mkdir -p ${CONFIG_BASE_DIR}/deluge
mkdir -p ${CONFIG_BASE_DIR}/lidarr
mkdir -p ${CONFIG_BASE_DIR}/notifiarr
mkdir -p ${CONFIG_BASE_DIR}/nzbget
mkdir -p ${CONFIG_BASE_DIR}/overseerr
mkdir -p ${CONFIG_BASE_DIR}/plex
mkdir -p ${CONFIG_BASE_DIR}/portainer
mkdir -p ${CONFIG_BASE_DIR}/prowlarr
mkdir -p ${CONFIG_BASE_DIR}/radarr
mkdir -p ${CONFIG_BASE_DIR}/readarr
mkdir -p ${CONFIG_BASE_DIR}/sabnzbd
mkdir -p ${CONFIG_BASE_DIR}/sonarr
mkdir -p ${CONFIG_BASE_DIR}/syncthing
mkdir -p ${CONFIG_BASE_DIR}/tailscale
mkdir -p ${CONFIG_BASE_DIR}/tautulli
mkdir -p ${CONFIG_BASE_DIR}/transmission-openvpn
mkdir -p ${CONFIG_BASE_DIR}/unpackerr
mkdir -p ${CONFIG_BASE_DIR}/watchtower
```

## Environment Variables

In `environment_variables.env.example` are the environment variables to use for
the Docker Compose files. You will want to copy this to `.env` and modify the
file according to the values for your setup. Here are all the environment
variables and their purpose:

- `BOOKS_DIR_LOCAL`: The directory to keep Book files that are organized
  locally on disk (i.e. `/volume1/books`)
- `BOOKS_DIR_RELATIVE`: The directory to keep Book files that are organized
  *relative* to the container (i.e. **not** the actual location of the files on
  disk e.g. `/books` => `${BOOKS_DIR_LOCAL}` e.g. `/books` => `/volume1/books`)
- `CONFIG_BASE_DIR`: The base directory where each of the containers will have
  their configurations persistently stored between reboots, restarts, etc. (e.g.
  `/volume1/docker`... and will serve the container the directory (e.g.
  *Transmission OpenVPN* => `/volume1/docker`/transmission-openvpn, *Plex* =>
  `/volume1/docker`/plex, *Tailscale* => `/volume1/docker`/tailscale, etc.) =>
  (e.g. Transmission OpenVPN => `transmission-openvpn`, Plex => `plex`,
  Tailscale => `tailscale`, etc.)
- `CONFIG_DATA_DIR`: The directory for each of the containers to store their
  data persistently stored between reboots, restarts, etc. (e.g.
  `/volume1/Data`... will serve the data directories for e.g. `complete`,
  `incomplete`, and `watch` for Transmission)
- `DATA_DIR_COMPLETE_LOCAL`: The directory to keep completely downloaded
  files locally on disk (i.e. `/volume1/data/complete`)
- `DATA_DIR_COMPLETE_RELATIVE`: The directory to keep completely downloaded
  files *relative* to the container (i.e. **not** the actual location of the
  files on disk e.g. `/complete` => `${DATA_DIR_COMPLETE_LOCAL}` e.g.
  `/complete` => `/volume1/data/complete`)
- `DATA_DIR_INCOMPLETE_LOCAL`: The directory to keep incompletely downloaded
  files locally on disk (i.e. `/volume1/data/incomplete`)
- `DATA_DIR_INCOMPLETE_RELATIVE`: The directory to keep incompletely downloaded
  files *relative* to the container (i.e. **not** the actual location of the
  files on disk e.g. `/incomplete` => `${DATA_DIR_INCOMPLETE_LOCAL}` e.g.
  `/incomplete` => `/volume1/data/watch`)
- `DATA_DIR_WATCH_LOCAL`: The directory to keep files to be watched
  locally on disk (i.e. `/volume1/data/watch`)
- `DATA_DIR_WATCH_RELATIVE`: The directory to keep files to be watched
  *relative* to the container (i.e. **not** the actual location of the files on
  disk e.g. `/watch` => `${DATA_DIR_WATCH_LOCAL}` e.g. `/watch` =>
  `/volume1/data/watch`)
- `GATEWAY_IP`: Gateway IP for the Docker Compose subnet (e.g. `172.16.0.1`)
- `HEALTH_CHECK_HOST`: Hostname to use for VPN health checks (e.g.:
  `google.com`)
- `IP_RANGE`: IP range for the Docker Compose subnet
- `LAN_NETWORK`: Private IP network the Docker Compose subnet is attached to on
  you private LAN (e.g. `192.168.0.0/24` => `192.168.0.1`: router, `192.168.0.5`:
  NAS machine, `192.168.0.88`: Laptop, etc.)
- `MOVIES_DIR_LOCAL`: The directory to keep Movie files that are organized
  locally on disk (i.e. `/volume1/movies`)
- `MOVIES_DIR_RELATIVE`: The directory to keep Movie files that are organized
  *relative* to the container (i.e. **not** the actual location of the files on
  disk e.g. `/movies` => `${MOVIES_DIR_LOCAL}` e.g. `/movies` =>
  `/volume1/movies`)
- `MUSIC_DIR_LOCAL`: The directory to keep Music files that are organized
  locally on disk (i.e. `/volume1/music`)
- `MUSIC_DIR_RELATIVE`: The directory to keep Music files that are organized
  *relative* to the container (i.e. **not** the actual location of the files on
  disk e.g. `/music` => `${MUSIC_DIR_LOCAL}` e.g. `/music` => `/volume1/music`)
- `NAME_SERVERS`: The DNS servers to use when the VPN is connected
- `NZBGET_WEBUI_PASSWORD`: NZBGet Password for the Web UI
- `NZBGET_WEBUI_USERNAME=admin`: NZBGet Username for the Web UI
- `OPENVPN_CONFIG`: Transmission OpenVPN configuration file(s) to use (e.g.
  `us_west.ovpn` => `us_west` or to use more than one file:
  `us_west,us_california,us_east`)
- `OPENVPN_OPTS`: Transmission OpenVPN optional arguments
- `PLEX_CLAIM=`: The Plex claim ID received from <https://plex.tv/claim> when
  first starting the Plex service
- `SUBNET`: The subnet for the Docker Compose environment (e.g. `172.16.0.0/16`)
- `TAILSCALE_HOSTNAME`: The hostname of this tailscale instance (e.g.
  `my-nas-server`)
- `TAILSCALE_STATE_ARG`: The Tailscale argument for the state argument variable
  (e.g. `"mem:"`)
- `TS_AUTH_KEY`: The Tailscale authorization key from
  [Tailscale.com > Settings > Personal Settings > Keys](https://login.tailscale.com/admin/settings/keys)
- `TV_DIR_LOCAL`: The directory to keep TV show files that are organized
  locally on disk (i.e. `/volume1/tv`)
- `TV_DIR_RELATIVE`: The directory to keep TV show files that are organized
  *relative* to the container (i.e. **not** the actual location of the files on
  disk e.g. `/tv` => `${TV_DIR_LOCAL}` e.g. `/tv` => `/volume1/tv`)
  `/volume1/movies`)
- `UN_LIDARR_0_API_KEY`: Lidarr API key
- `UN_RADARR_0_API_KEY`: Radarr API key
- `UN_READARR_0_API_KEY`: Readarr API key
- `UN_SONARR_0_API_KEY`: Sonarr API key
- `VPN_PASS`: VPN password for your VPN provider
- `VPN_USER`: VPN username for your VPN provider
- `WATCHTOWER_NOTIFICATION_URL`: (optional) Watchtower Webhook Notification URL

## Transmission OpenVPN Setup

Even if you are not using Transmission for your torrent download client (I
personally use Deluge) I found the container comes up much faster than the
DelugeVPN project and it also is more stable of a connection between both the
containers using it as well as with the VPN provider. Additionally the
Transmission OpenVPN project is more friendly for selecting multipule VPN
servers as well as configuration. Below is an example with Private Internet
Access but the project supports *many* other services (which again is also why
I did not like the DelugeVPN project). You'll want to follow similar steps for
whichever provider you have chosen. However, the end goal is to get all of your
OpenVPN `.ovpn` files downloaded in the Transmission OpenvVPN containers
configuration directory under the directory `openvpn`:

```shell
pushd ${CONFIG_BASE_DIR}/transmission-openvpn/
  curl -O <https://www.privateinternetaccess.com/openvpn/openvpn.zip>
  unzip openvpn.zip
popd
```

### IMPORTANT NOTE

You should know for this project I believe because I am running an older kernel
on my host OS I am getting the following error:

```shell
ERROR: problem running ufw-init
iptables v1.8.7 (nf_tables): Could not fetch rule set generation id: Invalid argument`iptables v1.8.7 (nf_tables): Could not fetch rule set
generation id: Invalid argument"`
```

Therefore,
as [suggested in this GitHub issue](https://github.com/P0cL4bs/wifipumpkin3/issues/140#issuecomment-1294201623)
I have added the following entrypoint script:

```shell
# NOTE: This is for Private Internet Access customers only but the same general
# principle will apply when you find your OpenVPN `.ovpn` files:
pushd ${CONFIG_BASE_DIR}/transmission-openvpn/
  # This is needed because I am rnning on an old kernel
  tee -a entrypoint.sh<<EOF
#!/bin/sh
update-alternatives --set iptables /usr/sbin/iptables-legacy
exec "\$@"
EOF
  chmod +x entrypoint.sh
popd
```

For this addition in the `docker-compose-downloaders.yaml` file, as you will
find, I have added the one line:

```docker-compose
    command: [ "dumb-init", "/entrypoint.sh", "/etc/openvpn/start.sh" ]
```

As well as the following mounted volume:

```docker-compose
    volumes:
    ...
      - ${CONFIG_BASE_DIR}/transmission-openvpn/entrypoint.sh:/entrypoint.sh`
    ...
```

**This means you will need to add the aformentioned IMPORTANT NOTE steps I have
added just above here if you want this to work.** It should work just as fine
but will be running an older version of `iptables` (i.e. `iptables-legacy`).
