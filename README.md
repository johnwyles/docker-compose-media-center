# docker-compose-media-center

This respository contains the Docker compose files for standing up an entire Servarr
stack along with a other supporting tools and services best suited for an at-home
NAS or media center

**Table of contents:**
- [docker-compose-media-center](#docker-compose-media-center)
  - [Useful Resources To Have On-hand](#useful-resources-to-have-on-hand)
  - [Docker Compose Files and the Applications and Services Within](#docker-compose-files-and-the-applications-and-services-within)
    - [`portainer/docker-compose-portainer.yaml`](#portainerdocker-compose-portaineryaml)
    - [`docker-compose-crypto.yaml` \[OPTIONAL\]](#docker-compose-cryptoyaml-optional)
    - [`docker-compose-ops.yaml`](#docker-compose-opsyaml)
    - [`docker-compose-tools.yaml`](#docker-compose-toolsyaml)
    - [`docker-compose-downloaders.yaml`](#docker-compose-downloadersyaml)
    - [`docker-compose-servarr.yaml`](#docker-compose-servarryaml)
    - [`docker-compose-photos.yaml`](#docker-compose-photosyaml)
    - [`docker-compose-media-players.yaml`](#docker-compose-media-playersyaml)
    - [`docker-compose-kometa.yaml`](#docker-compose-kometayaml)
    - [`docker-compose-homeassistant.yaml`](#docker-compose-homeassistantyaml)
  - [Installation and Setup](#installation-and-setup)
  - [Directory Structure](#directory-structure)
  - [Environment Variables](#environment-variables)
  - [TODO](#todo)
    - [TODO: Add Gluetun Instructions](#todo-add-gluetun-instructions)


## Useful Resources To Have On-hand

Here are some useful resources to keep on hand as you go through the instuctions
below for setting up the services contained in the Docker compose files in this
repository:

- [Servarr Wiki](https://wiki.servarr.com/): Best place to start to get to
understand some of the services that are setup in the set of Docker compose
files in this repository, what they are, and how to set them up and troubleshoot
them.
- [LinuxServer.io](https://docs.linuxserver.io/general/container-execution/):
The How To section here is full of a lot of quick introductory material for not
only Docker, Docker Compose, and other good material on what forms the foundation
of many of the concepts you will need to understand utilizing the Docker compose
files in this repository as well as following along with the instuctions below.
Not only that but [LinuxServer.io](https://www.linuxserver.io/) has built and
maintains many of the Docker images and formed, to a large extent, the formatting
and structure of the Docker compose files in this repository.
- [TRaSH Guides](https://trash-guides.info/): Collected here are a great set of
documents that somewhat explain the services but is more of an excellent source
for general best practices as well as offering tips and providing help for some
edge cases you may encounter.

## Docker Compose Files and the Applications and Services Within

Below are a list of all of the Docker compose files that setup a Servarr stack,
some associated assisting tools, media players, and also miscellaneous self-hosted
tools you can skip and and/or remove from as well as mostly links to their
associated Docker image sources, GitHub repositories, or

### `portainer/docker-compose-portainer.yaml`

- [Portainer](https://hub.docker.com/r/portainer/portainer) Portainer for
  managing the remaining "Stacks" (docker-compose files).

### `docker-compose-crypto.yaml` [OPTIONAL]

**NOTE:** I do not set this up in later steps below as I highly doubut you will want
to peg your CPU and run this utility alongside what this guide was meant to assit
in setting up. You will likely find it completely unnessesary but feel free to
contact me if you would like more information.

- [rana](https://github.com/grunch/rana): A tool for finding _vanity_ public
  keys used with `nostr`.
- [vanity-age](https://github.com/johnwyles/vanity-age): A rewrite in a fork I
  did of [vanity-age](https://github.com/sobaq/vanity-age) for finding _vanity_
  public `age` keys.

### `docker-compose-ops.yaml`
- [cAdvisor](https://github.com/google/cadvisor): A tool for monitoring Docker
  containers, images, system resources, etc.
- [Grafana](https://github.com/grafana/grafana): A tool for visualizing metrics.
- [iperf]([iperf3](https://github.com/esnet/iperf)): A utility used to test
  speeds between two machines. Both a client and server tool in one. In this
  case we will run the container in server mode.
- [Ofelia](https://github.com/mcuadros/ofelia): A tool for scheduling cron jobs
  in Docker containers.
- [Prometheus](https://github.com/prometheus/prometheus): A tool for monitoring
  metrics and statistics.
- [Prometheus SNMP Exporter](): An important utility for collection device
  statistics for the Synolog NAS
- [Redis](https://github.com/redis/redis): A tool for caching and storing data.
- [Watchtower](https://github.com/containrrr/watchtower): Watchtower for watching
  updates to docker images and pulling them down.

### `docker-compose-tools.yaml`
- [Paperless-NGX](https://github.com/paperless-ngx/paperless-ngx): A tool for 
  organizing documents and files.
- [SearXNG](https://github.com/searxng/searxng-docker): Utility for an internet
  metasearch engine that aggregates search results.
- [Syncthing](https://github.com/linuxserver/docker-syncthing): Utility for
  syncing files across machines.
- [Tailscale](https://hub.docker.com/r/tailscale/tailscale): Private VPN to
  chain together your home network and other networks and machines for sharing
  services and files.

### `docker-compose-downloaders.yaml`

**NOTE:**: For posterity's sake I left in SABnzbdVPN and Transmission-OpenVPN
but they are commented out in the `docker-compose-downloaders.yaml` file. I
found using [glueun](`https://github.com/qdm12/gluetun`) mentioned above to be a
better utility to operate as a VPN killswitch universally I could plug any
container I wanted to up to it.

- ~~[Deluge](https://github.com/linuxserver/docker-deluge): Deluge for torrent
  downloads.~~
- [Exportarr](https://github.com/onedr0p/exportarr): Exportarr for metrics (qBittorrent).
- [Gluetun](https://github.com/qdm12/gluetun): Utility for a base VPN killswitch
  container that the downloading containers filter through when downloading.
- ~~[NZBGet](https://github.com/linuxserver/docker-nzbget): NZBGet for Usenet
  downloads.~~
- [qBitorrent](https://docs.linuxserver.io/images/docker-qbittorrent/): A
  sophisticated and battle tested bittorrent client.
- [SABnzbd](https://github.com/linuxserver/docker-sabnzbd) SABnzbd for Usenet
  downloads.
- ~~[SABnnzdVPN](https://github.com/binhex/arch-sabnzbdvpn) SABnzbd with OpenVPN
  for torrent downloads which includes a VPN killswitch to stop downloading on
  loss of a VPN connection.~~
- ~~[Tranismission](https://github.com/linuxserver/docker-transmission)
  Transmission for torrent downloads.~~
- ~~[Transmission-OpenVPN](https://github.com/haugene/docker-transmission-openvpn):
  Transmission with OpenVPN for torrent downloads which includes a VPN killswitch
  to stop downloading on loss of a VPN connection.~~
- [Unpackerr](https://hub.docker.com/r/golift/unpackerr): Unpackerr for any
  unpacking in post-processing after download.

### `docker-compose-servarr.yaml`

- [Bazarr](https://github.com/linuxserver/docker-bazarr): Bazarr for adding
  Subtitles to media found in Radarr and Sonarr.
- [Exportarr](https://github.com/onedr0p/exportarr): Exportarr for metrics.
- [Mylar3](https://github.com/mylar3/mylar3): Mylar3 for Comic Books.
- [Lidarr](https://github.com/linuxserver/docker-lidarr): Lidarr for Music.
- [Notifiarr](https://hub.docker.com/r/golift/notifiarr): Notifiarr for Discord
  and Webhooks.
- [Prowlarr](https://github.com/linuxserver/docker-prowlarr): Prowlarr for
  adding Indexers to Stararr services.
- [Radarr](https://github.com/linuxserver/docker-radarr): Radarr for Movies.
- [Readarr](https://github.com/linuxserver/docker-readarr): Readarr for eBooks
  and Audiobooks.
- [Sonarr](https://github.com/linuxserver/docker-sonarr): Sonarr for TV series
  and shows.

### `docker-compose-photos.yaml`

- [Immich](https://github.com/immich-app/immich): The new photo management
  utility on the block for managing photos, videos, and other assorted media
  with object, color, face, and other recognition and tagging. Fairly unstable
- [Photo Prism](https://github.com/photoprism/photoprism): A  battle tested and
  more stable photo, video, and assorted media organization utility for manging
  media

### `docker-compose-media-players.yaml`

- ~~[Jellyfin](https://github.com/linuxserver/docker-jellyfin): Jellyfin media
  center for viewing and playing all of your media.~~
- ~~[Jellyseerr](https://hub.docker.com/r/fallenbagel/jellyseerr): Jellyseerr
  (Overseerr fork) for browsing and discovering of new media.~~
- [Plex](https://github.com/linuxserver/docker-plex): Plex media center for
  viewing and playing all of your media.
- ~~[Tautulli](https://github.com/linuxserver/docker-tautulli): Tautulli for Plex
  statistics and monitoring.~~
- ~~[Overseerr](https://github.com/linuxserver/docker-overseerr): Overseer for
  browsing and discovering of new media.~~

### `docker-compose-kometa.yaml`

- [Kometa](https://github.com/Kometa-Team/Kometa): A fairly complex piece of
  software to render badges, evaluate audience/user/critic ratings, creat/manage
  collections, and a whole host of many other things. The best way to get
  started is probably seeing some explanations and visuals on the on the 
  [Kometa wiki page](https://metamanager.wiki/en/latest/).

### `docker-compose-homeassistant.yaml`

- [Home Assistant](https://www.home-assistant.io/installation/linux#docker-compose):
  This is not only the popular open-source home automation suite, Home Assistant,
  but also a number of tools and services that compliment it. This file will
  likely be *heavily* modified from the boiler plate file included here. I have
  made extensive comments as to what each service is and I assure you your setup
  will largely differ. This is just a collection of how I have set up my home
  automation using Home Assistant and hopefully can be of some help to you. I
  will continue to update it as genericly as I can so it hopefully is of
  greater use as a jumping off point.

## Installation and Setup

1. Install Docker
2. Create a `media` network with:

    ```shell
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
many of the services and applications. **IMPORTANT NOTE:** Complete the few
values
necessary and update the `.env` files as you go. You must complete each **must**
service _before_ moving to the next step. This means your `.env` file does not
need to have ALL the correct values from the start. Just as long as you have the
values Docker compose needs then everything should proceed smoothly:
    - [Directory Structure](#directory-structure)
    - [Environment Variables](#environment-variables)
1. Run these `docker compose` commands in order (**Note:** there are few
  `depends_on` throughout though internally the `docker-compose-downloaders.yaml`
  file has containers which are dependent on another - i.e. `sabnzbd` and `deluge`
  use the `gluetun` network service to utilize it's utility as a VPN killswitch):
    - First thing we do is make sure the portainer Docker compose file is in the
      `portainer/` directory. This is because Portainer is weird and launches
      with a stack named after the parent directory to the image it is launch is
      called. This way in our case it will be `portainer` now:
      - `cd portainer/`
      - `docker compose --file docker-compose-portainer.yaml --env-file ../.env up --detach`
    - And now for the rest:
      - `cd ../`
      - `docker compose --file docker-compose-ops.yaml --env-file .env up --detach`
      - `docker compose --file docker-compose-tools.yaml --env-file .env up --detach`
      - `docker compose --file docker-compose-downloaders.yaml --env-file .env up --detach`
      - `docker compose --file docker-compose-media-players.yaml --env-file .env up --detach`
      - `docker compose --file docker-compose-photos.yaml --env-file .env up --detach`
      - `docker compose --file docker-compose-servarr.yaml --env-file .env up --detach`
    - Also if you'd like to setup Kometa you'll want to add a cron entry you
      run periodically (around every 24-96 hours) with the following command or
      the alternative listed afterwards:
      - `docker-compose --file docker-compose-kometa.yaml --env-file .env up kometa --detach`
      - Open `jobs.ini` in `ofelia` and add the following:
        - ```ini
          [job-exec "kometa daily"]
            schedule = 45 11 * * *
            container = kometa
            command =  photoprism index --cleanup
            no-overlap = true
          ```
    - If you'd also like to setup Home Assistant you'll want to run that with
      the following command:
       - `docker-compose --file docker-compose-homeassistant.yaml --env-file .env up --detach`


## Directory Structure

In order to start you need to create several directories to house the
configurations for each service and where the Docker container will persist
configurations locally between reboots, restarts, etc. This maps directly to the
`CONFIG_BASE_DIR` directory in the `.env` file that we will get to later.

```shell
export CONFIG_BASE_DIR=/volume1/services # substitute for your path/preferences
mkdir -p ${CONFIG_BASE_DIR}
```

This sets the `CONFIG_BASE_DIR` directory that will store all of the subsequent
directories for their Docker configurations. **NOTE:** I have marked items below
I consider _optional_ but note that the files included here _will_ install them
all unless you have commented them out as well. If you do not every directory below
and it's subsequent service are later set up to be running and available to use.
Whether you use them or not you may find you would like to try them later so my
recommendation is to leave them as is and you can return later to remove them as
you require. If you do skip some of the services marked `optional` in creating
the directories below (highlighed by the `# optional ...` comments below then do
your best to find the [environment variables](#environment-variables)) and comment
those out as well (to keep down the noise in your `.env` file. To create each of
the directories run the following after the command above:

```shell
mkdir -p ${CONFIG_BASE_DIR}/bazarr
# mkdir -p ${CONFIG_BASE_DIR}/caddy
mkdir -p ${CONFIG_BASE_DIR}/deluge
# mkdir -p ${CONFIG_BASE_DIR}/delugevpn
mkdir -p ${CONFIG_BASE_DIR}/gluetun
mkdir -p ${CONFIG_BASE_DIR}/grafana/database
mkdir -p ${CONFIG_BASE_DIR}/grafana/snmp
mkdir -p ${CONFIG_BASE_DIR}/grafana/storage
mkdir -p ${CONFIG_BASE_DIR}/immich/cache
mkdir -p ${CONFIG_BASE_DIR}/immich/database
mkdir -p ${CONFIG_BASE_DIR}/jellyfin
mkdir -p ${CONFIG_BASE_DIR}/jellyseerr
mkdir -p ${CONFIG_BASE_DIR}/kometa
mkdir -p ${CONFIG_BASE_DIR}/lidarr
mkdir -p ${CONFIG_BASE_DIR}/mylar3
mkdir -p ${CONFIG_BASE_DIR}/notifiarr
# mkdir -p ${CONFIG_BASE_DIR}/nzbget
mkdir -p ${CONFIG_BASE_DIR}/ofelia
mkdir -p ${CONFIG_BASE_DIR}/overseerr
mkdir -p ${CONFIG_BASE_DIR}/paperless-ngx
mkdir -p ${CONFIG_BASE_DIR}/photoprism/database
mkdir -p ${CONFIG_BASE_DIR}/photoprism/storage
mkdir -p ${CONFIG_BASE_DIR}/plex
mkdir -p ${CONFIG_BASE_DIR}/portainer
mkdir -p ${CONFIG_BASE_DIR}/promethues/config
mkdir -p ${CONFIG_BASE_DIR}/promethues/data
mkdir -p ${CONFIG_BASE_DIR}/prowlarr
mkdir -p ${CONFIG_BASE_DIR}/qbittorrent
mkdir -p ${CONFIG_BASE_DIR}/radarr
mkdir -p ${CONFIG_BASE_DIR}/readarr
mkdir -p ${CONFIG_BASE_DIR}/sabnzbd
# mkdir -p ${CONFIG_BASE_DIR}/sabnzbdvpn
mkdir -p ${CONFIG_BASE_DIR}/searxng
mkdir -p ${CONFIG_BASE_DIR}/sonarr
mkdir -p ${CONFIG_BASE_DIR}/syncthing
mkdir -p ${CONFIG_BASE_DIR}/tailscale
mkdir -p ${CONFIG_BASE_DIR}/tautulli
mkdir -p ${CONFIG_BASE_DIR}/transmission
# mkdir -p ${CONFIG_BASE_DIR}/transmission-openvpn
```

Copy the resources for **Ofelia** and **Prometheus** to their respective
directories:
```shell
cp resources/ofelia/jobs.ini ${CONFIG_BASE_DIR}/ofelia/
cp resources/prometheus/prometheus.yaml ${CONFIG_BASE_DIR}/prometheus/config/
```

For **Grafana** you will want to import the dashboard from
`reources/grafana/servarr_dashboard.json` into the utility through the UI after
the service has been started.

## Environment Variables

In `environment_variables.env.example` are the environment variables to use for
the Docker Compose files. You will want to copy this to `.env` and modify the
file according to the values for your setup. Here are all the environment
variables and their purpose:

- `BAZARR_API_KEY`: The API key for Bazarr
- `BOOKS_DIR_LOCAL`: The directory to keep Book files that are organized
  locally on disk (i.e. `/volume1/books`)
- `BOOKS_DIR_RELATIVE`: The directory to keep Book files that are organized
  _relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/books` => `${BOOKS_DIR_LOCAL}` e.g. `/books` => `/volume1/books`)
- `COMICS_DIR_LOCAL`: The directory to keep Comic Book files that are organized
  locally on disk (i.e. `/volume1/books`)
- `COMICS_DIR_RELATIVE`: The directory to keep Comic Book files that are
  organized _relative_ to the container (i.e. **not** the actual location of the
  files on disk e.g. `/books` => `${BOOKS_DIR_LOCAL}` e.g. `/comics` =>
  `/volume1/comics`)
- `CONFIG_BASE_DIR`: The base directory where each of the containers will have
  their configurations persistently stored between reboots, restarts, etc. (e.g.
  `/volume1/docker`)... and will serve the container the directory (e.g.
  _Deluge_ => `/volume1/docker`/deluge, _Plex_ =>
  `/volume1/docker`/plex, _Tailscale_ => `/volume1/docker`/tailscale, etc.) =>
  (e.g. Deluge => `deluge`, Plex => `plex`,
  Tailscale => `tailscale`, etc.)
- `DOCUMENTS_DIR_LOCAL`: The directory of locally stored Document files
- `DOWNLOADS_DIR_COMPLETE_LOCAL`: The directory to keep completely downloaded
  files locally on disk (i.e. `/volume1/Downloads/complete`)
- `DOWNLOADS_DIR_COMPLETE_RELATIVE`: The directory to keep completely downloaded
  files _relative_ to the container (i.e. **not** the actual location of the
  files on disk e.g. `/complete` => `${DOWNLOADS_DIR_COMPLETE_LOCAL}` e.g.
  `/complete` => `/volume1/Downloads/complete`)
- `DOWNLOADS_DIR_INCOMPLETE_LOCAL`: The directory to keep incompletely downloaded
  files locally on disk (i.e. `/volume1/Downloads/incomplete`)
- `DOWNLOADS_DIR_INCOMPLETE_RELATIVE`: The directory to keep incompletely
  downloaded files _relative_ to the container (i.e. **not** the actual location
  of the files on disk e.g. `/incomplete` => `${DOWNLOADS_DIR_INCOMPLETE_LOCAL}`
  e.g. `/incomplete` => `/volume1/Downloads/watch`)
- `DOWNLOADS_DIR_WATCH_LOCAL`: The directory to keep files to be watched
  locally on disk (i.e. `/volume1/Downloads/watch`)
- `DOWNLOADS_DIR_WATCH_RELATIVE`: The directory to keep files to be watched
  _relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/watch` => `${DOWNLOADS_DIR_WATCH_LOCAL}` e.g. `/watch` =>
  `/volume1/Downloads/watch`)
- `GATEWAY_IP`: Gateway IP for the Docker Compose subnet (e.g. `172.16.0.1`)
- `GRAFANA_DATABASE_NAME`: The name for the Grafana database
- `GRAFANA_DATABASE_USER`: THe user for the Grafana database
- `GRAFANA_DATABASE_PASSWORD`: THe password for the Grafana database
- `HEALTH_CHECK_HOST`: Hostname to use for VPN health checks (e.g.:
  `google.com`)
- `HOMEASSISTANT_PGID`:  The group ID for the running process in the container
  environment (e.g. `1000`)
- `HOMEASSISTANT_PUID`: The user ID for the running process in the container
  environment (e.g. `1000`)
- `HOMEASSISTANT_CONFIG_BASE_DIR`: The _base_ directory where each of the
  containers for and support Home Assistant will have their configurations
  persistently stored between reboots, restarts, etc. (e.g.
  `/volume1/homeassistant-docker`)... and will serve the container the directory
  (e.g. _NodeRED_ => `/volume1/homeassistant-docker`/nodered, _ZigBee_ =>
  `/volume1/homeassistant-docker`/zigbee, _Home Assistant_ => 
  `/volume1/homeassistant-docker`/homeassistant, etc.) =>  (e.g. NodeRED =>
  `nodered`, ZigBee => `zigbee`, Home Assistant => `homeassistant`, etc.). So,
  in this example, it is simply the home where each of these containers
  configurations will live: `/volume1/homeassistant-docker`
- `HOMEASSISTANT_DATA_DIR_1_LOCAL`: The directory to of data files we would like
  exposed to Home Assistant locally on disk (i.e. `/tmp`)
- `HOMEASSISTANT_DATA_DIR_2_LOCAL`: The directory to of data files we would like
  exposed to Home Assistant locally on disk (i.e. `/etc`)
- `HOMEASSISTANT_DATA_DIR_3_LOCAL`: The directory to of data files we would like
  exposed to Home Assistant locally on disk (i.e. `/mnt`)
- `HOMEASSISTANT_DATA_DIR_1_RELATIVE`: The directory to we house data files
  _relative_ to the container (i.e. **not** the actual location of the
  files on disk e.g. `/foo_tmp` => `${HOMEASSISTANT_DATA_DIR_1_LOCAL}` e.g.
  `/foo_tmp` => `/tmp`)
- `HOMEASSISTANT_DATA_DIR_2_RELATIVE`: The directory to we house data files
  _relative_ to the container (i.e. **not** the actual location of the
  files on disk e.g. `/etc_tmp` => `${HOMEASSISTANT_DATA_DIR_2_LOCAL}` e.g.
  `/etc_tmp` => `/etc`)
- `HOMEASSISTANT_DATA_DIR_3_RELATIVE`: The directory to we house data files
  _relative_ to the container (i.e. **not** the actual location of the
  files on disk e.g. `/mnt_tmp` => `${HOMEASSISTANT_DATA_DIR_3_LOCAL}` e.g.
  `/mnt_tmp` => `/mnt`)
- `IMMICH_DB_DATABASE_NAME`: The name for the Immich database
- `IMMICH_DB_PASSWORD`: A password for the Immich database
- `IMMICH_DB_USERNAME`: A username for the Immich database
- `IMMICH_VERSION`: The version of Immich to use (default: `release`)
- `IP_RANGE`: IP range for the Docker Compose subnet
- `KOMETA_PLEX_TOKEN`: The Plex claim ID from above but without the `claim-`
  prefix
- `KOMETA_PLEX_URL`: The internal Docker URL for the plex container for use by
  Kometa (previously: Plex Meta Manager) (e.g. (and probably should not change)
  `http://plex:32400`)
- `LAN_NETWORK`: Private IP network the Docker Compose subnet is attached to on
  you private LAN (e.g. `192.168.0.0/24` => `192.168.0.1`: router,
  `192.168.0.5`: NAS machine, `192.168.0.88`: Laptop, etc.)
- `LIDARR_API_KEY`: The API key for Lidarr
- `MOVIES_DIR_LOCAL`: The directory to keep Movie files that are organized
  locally on disk (i.e. `/volume1/movies`)
- `MOVIES_DIR_RELATIVE`: The directory to keep Movie files that are organized
  _relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/movies` => `${MOVIES_DIR_LOCAL}` e.g. `/movies` =>
  `/volume1/movies`)
- `MUSIC_DIR_LOCAL`: The directory to keep Music files that are organized
  locally on disk (i.e. `/volume1/music`)
- `MUSIC_DIR_RELATIVE`: The directory to keep Music files that are organized
  _relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/music` => `${MUSIC_DIR_LOCAL}` e.g. `/music` => `/volume1/music`)
- `NAME_SERVERS`: The DNS servers to use when the VPN is connected
- ~~`NZBGET_WEBUI_PASSWORD`: NZBGet Password for the Web UI~~
- ~~`NZBGET_WEBUI_USERNAME=admin`: NZBGet Username for the Web UI~~
- `OPENVPN_CONFIG`: Transmission OpenVPN configuration file(s) to use (e.g.
  `us_west.ovpn` => `us_west` or to use more than one file:
  `us_west,us_california,us_east`)
- `OPENVPN_OPTS`: Transmission OpenVPN optional arguments (default: null)
- `OPENVPN_PROVIDER`: Transmission OpenVPN Provider (e.g. `PIA` for Private
  Internet Access VPN, etc.)
- `PERSONAL_DIR_LOCAL`: The directory where Personal videos files that are
  organized locally on disk (i.e. `/volume1/personal/videos`)
- `PERSONAL_DIR_RELATIVE`: The directory where Personal videos files that are
  organized _relative_ to the container (i.e. **not** the actual location of the
  files on disk e.g. `/personal` => `${PERSONAL_DIR_LOCAL}` e.g. `/personal` =>
  `/volume1/personal/videos`)
- `PGID`: The group ID for the running process in the container environment
  (e.g. `1000`)
- `PHOTOPRISM_USERNAME`: The username for the PhotoPrism service
- `PHOTOPRISM_PASSWORD`: The passowrd for the PhotoPrism service
- `PHOTOPRISM_DATABASE_NAME`: The name for the PhotoPrism database
- `PHOTOPRISM_DATABASE_USER`: The database user for the PhotoPrism database
- `PHOTOPRISM_DATABASE_PASSWORD`: The database password for the PhotoPrism
  database (e.g. `password`)
- `PHOTOPRISM_DATABASE_ROOT_PASSWORD`: The database root password for the
  PhotoPrism database
- `PHOTOS_DIR_LOCAL`: The directory to photo file originals are stored for use
  by the photo services herein (e.g. PhotoPrism, Immich, etc.)
- `PLEX_CLAIM`: The Plex claim ID received from <https://plex.tv/claim> when
  first starting the Plex service
- `PORTAINER_PASSWORD`: An http password encoded string (see this tool:
  [HTPasswd Generator](https://www.web2generators.com/apache-tools/htpasswd-generator))
  to use for the Portainer docker instance we create
- `PRIVATE_INTERNET_ACCESS_VPN_PORT_FORWARDING`: Gluetun VPN killswitch setting
  for port forward with Private Internet Access (i.e. `on` or `off`)
- `PROWLARR_API_KEY`: The API key for Prowlarr
- `PUID`: The user ID for the running process in the container environment
  (e.g. `1000`)- ~~`RANA_ARGUMENTS`: Arguments to pass in `rana` on execution to
  find a prefixed sub-string(s) (e.g. "-n=j0hnwyles,j0hnwyl3s -c 4")~~
- `READARR_API_KEY`: The API key for Readarr
- `RADARR_API_KEY`: The API key for Radarr
- `SABNZBD_API_KEY`: The API key for Sabnzbd
- `SERVER_COUNTRIES`: Gluetun VPN killswitch setting for the regions to use
- `SEARXNG_HOSTNAME`: Hostname to reference for SearXNG internally
- `SONARR_API_KEY`: The API key for Sonarr
  (e.g. `Switzerland,Estonia,Iceland,Panama,Romania`)
- `SUBNET`: The subnet for the Docker Compose environment (e.g. `172.16.0.0/16`)
- `SYNCTHING_MOUNT_DIR_1_LOCAL`: The directory of a path locally that you would
like to have Syncthing sync with other Syncthing instances (i.e.
`/volume1/sync`)
- `SYNCTHING_MOUNT_DIR_1_RELATIVE`: The directory Syncthing will refer to
  locally
_relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/sync` => `${SYNCTHING_MOUNT_DIR_1_LOCAL}` e.g. `/sync` =>
  `/volume1/sync`)
- `SYNCTHING_MOUNT_DIR_2_LOCAL`: The directory of a path locally that you would
like to have Syncthing sync with other Syncthing instances (i.e.
`/volume1/some_other_directory`)
- `SYNCTHING_MOUNT_DIR_2_RELATIVE`: The directory Syncthing will refer to locally
_relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/some_other_directory` => `${SYNCTHING_MOUNT_DIR_1_LOCAL}` e.g.
  `/some_other_directory` =>
  `/volume1/some_other_directory`)
- `TAILSCALE_HOSTNAME`: The hostname of this tailscale instance (e.g.
  `my-nas-server`)
- `TAILSCALE_STATE_ARG`: The Tailscale argument for the state argument variable
  (e.g. `"mem:"`)
- `TRANSMISSION_PASS`: The default password to set for Tranmission account
  (default: `admin`)
- `TRANSMISSION_USER`: The account for access to Transmission (default: `admin`)
- `TS_ACCEPT_DNS`: Tailscale setting for DNS entries (default: `true`)
- `TS_AUTH_KEY`: The Tailscale authorization key from
  [Tailscale.com > Settings > Personal Settings > Keys](https://login.tailscale.com/admin/settings/keys)
- `TS_DEST_IP`: Tailscale setting for target IP (default: null)
- `TS_EXTRA_ARGS`: Extra arguments to pass to `tailscale up` (Recommended:
  `="--reset --advertise-exit-node --ssh"`)
- `TS_KUBE_SECRET`: Kubernetes secret if you are in a K8S cluster
- `TS_OUTBOUND_HTTP_PROXY_LISTEN`: Proxy settings if you have outbound proxy
  settings (default: null)
- `TS_ROUTES`: Tailscale routing (default: null)
- `TS_SOCKET`: Socket file for `tailscaled` (default: `/tmp/tailscaled.sock`)
- `TS_TAILSCALED_EXTRA_ARGS`: Extra arguments to pass to start of `tailscaled`
  (default: null)
- `TS_USERSPACE`: Userspace setting for Tailscale (default: null)
- `TS_SOCKS5_SERVER`: SOCKS5 settings (default: null)
- `TS_STATE_DIR`: Directory for tailscale storage state directory (default:
  `/var/lib/tailscale`)
- `TV_DIR_LOCAL`: The directory to keep TV show files that are organized
  locally on disk (i.e. `/volume1/tv`)
- `TV_DIR_RELATIVE`: The directory to keep TV show files that are organized
  _relative_ to the container (i.e. **not** the actual location of the files on
  disk e.g. `/tv` => `${TV_DIR_LOCAL}` e.g. `/tv` => `/volume1/tv`)
  `/volume1/movies`)
- `UN_LIDARR_0_API_KEY`: Lidarr API key
- `UN_RADARR_0_API_KEY`: Radarr API key
- `UN_READARR_0_API_KEY`: Readarr API key
- `UN_SONARR_0_API_KEY`: Sonarr API key
- ~~`VANITY_AGE_ARGUMENTS`: A RegEx that you would like to find at the beginning
  of a `age` public key (e.g. "\d+j0hn\d?wyles.*")~~
- `VPN_PASS`: VPN password for your VPN provider
- `VPN_SERVICE_PROVIDER`: Gluetun VPN service provider (e.g.
  `private internet access` for Private Internet Access VPN, `nordvpn` for
  NordVPN, etc.)
- `WATCHTOWER_HTTP_API_TOKEN`: A passphrase to use when pulling metrics for a
  tool like Promethus
- `WATCHTOWER_NOTIFICATION_URL`: A webhook URL to hit for Watchtower
  notifications
- `WATCHTOWER_POLL_INTERVAL`: Poll interval for Watchtower to check for images
- `VPN_USER`: VPN username for your VPN provider
- `WALLABAG_DATABASE_NAME`: The name for the Wallabag database
- `WALLABAG_DATABASE_PASSWORD`: The password for the Wallabag database
- `WALLABAG_DATABASE_ROOT_PASSWORD`: The root password for the Wallabag database
- `WALLABAG_DATABASE_USER`: The user for the Wallabag database
- `WALLABAG_DOMAIN_NAME`: The domain name for the Wallabag service
- `WALLABAG_FROM_EMAIL`: The email address to use for the "from" field in emails
- `WALLABAG_SERVER_NAME`: The server name for the Wallabag service
- `WATCHTOWER_NOTIFICATION_URL`: (optional) Watchtower Webhook Notification URL
- `WATCHTOWER_POLL_INTERVAL`: Interval for Watchtower to check for new container
  images (e.g. 21600 ("6 hours"))
- `WIREGUARD_PRIVATE_KEY`: Gluetun wireguard private key setting (Author note: I
  do regret if you have to go through setting this arduous process... Details
  here (for NordVPN at least):
  [Getting NordVPN WireGuard details](https://gist.github.com/bluewalk/7b3db071c488c82c604baf76a42eaad3)

## TODO

  Now personal notes to myself on some of that which remains:

- **P0:** Consider adding [photoprism](https://github.com/photoprism/photoprism)
- **P1:** Consider adding [Lemmy](https://blog.colic.io/2023/07/07/self-hosting-lemmy-a-step-by-step-guide-with-docker-compose/)
- **P1:** Add instructions for wiring everything together (with pictures?)
- **P1:** Add instructions for Gluetun containers to replace those found below
- **P2:** Add `ports` and whatever else from `docker-compose-*.yaml` files to be
  passed as ENV variables
- **P3:** See if SearXNG needs more tuning and spend some time with it
- **P3:** Add `rana` and `vanity-age` executables, Docker build instructions, and
  `docker compose` running instructions
- **P3:** Figure out searxng
- **P3:** Add documentation or maybe breakout Home Assistant

### TODO: Add Gluetun Instructions

```shell
pushd ${CONFIG_BASE_DIR}/transmission-openvpn/
  curl -O <https://www.privateinternetaccess.com/openvpn/openvpn.zip>
  unzip openvpn.zip
popd
```
