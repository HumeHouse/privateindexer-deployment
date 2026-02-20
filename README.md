# PrivateIndexer

### The ultimate way to share media between all your homelabbing friends

This repository contains centralized setup guides for server operators of a PrivateIndexer swarm.

## What is PrivateIndexer?

**PrivateIndexer** is a self-hosted private tracker and indexer designed for small, trusted groups who want to seamlessly and automatically share media (Linux ISOs etc.) with each other.

It allows you and your friends to form your own private torrent swarm — where **you are the release groups**. Instead of granting full library access through media servers, PrivateIndexer enables decentralized sharing using torrents generated directly from your existing media collection.

PrivateIndexer is primarily built for users running their own media stacks (e.g., Plex, Jellyfin, Emby, and the *arr suite) who want efficient, automated sharing between peers.

![PrivateIndexer stack image](privateindexer.png)

## How It Works

PrivateIndexer consists of three components:

- `server`
- `tracker`
- `client`

Each component has a clearly defined role within the swarm.

## Architecture Overview

### `server`

The `server` manages users and torrents.

Core functions:

- Accepts uploaded `.torrent` files from clients
- Stores and indexes torrent metadata
- Responds to Torznab search queries
- Serves torrent files to authorized users

The `server` acts as the indexer and API layer for the swarm.

---

### `tracker`

The `tracker` handles peer coordination.

Core functions:

- Maintains a record of peers per torrent
- Facilitates peer discovery within the swarm

> [!IMPORTANT]
> Each swarm should operate with **one `server` and one `tracker`**.
> Server operators are responsible for maintaining both components.

---

### `client`

The `client` is installed by trusted members of the swarm.

Core functions:

- Scans connected *arr applications (Radarr, Sonarr, Lidarr) for media
- Automatically generates torrents based on content type:
  - Single-file torrents (movies, episodes, individual tracks)
  - Multi-file torrents (seasons, full albums)
- Uploads generated torrents to the `server`
- Seeds content using an internal libtorrent-based engine
- Announces torrents to the `tracker`

Once torrents are indexed, users can:

1. Query the `server` via Torznab
2. Retrieve matching torrent files
3. Download content directly from peers in the swarm

## Getting started with the `server` & `tracker`

First, review the example [docker-compose-stack.yml file](docker-compose-stack.yml), then follow the guides for each component.

The [privateindexer-server guide](guides/SERVER.md) walks you through configuring and running the server.

Next, follow the [privateindexer-tracker guide](guides/TRACKER.md) to complete the tracker setup.

> [!NOTE]
> These guides assume you are using the provided MySQL and Redis Docker container configurations.

## Distribute the `client` to your swarm

Have your users review the example [docker-compose-client.yml file](docker-compose-client.yml)

They should then follow the [privateindexer-client guide](guides/CLIENT.md) to configure and run their client.

Each client will need the following information from you (the server operator):

1. Your externally accessible `server` URL - example: `https://indexer.humehouse.com` (same value as `EXTERNAL_SERVER_URL` on the server container)
2. Your externally accessible `tracker` URL - example: `https://tracker.humehouse.com`
3. Their unique API key (available in the server admin panel — see the bottom of the server guide)

Once configured, the client will scan media, upload torrents to the server, and automatically announce to the tracker.
