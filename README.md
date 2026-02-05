# PrivateIndexer

#### The ultimate way to share media between all your homelabbing friends

This repository contains centralized setup guides for server operators of a PrivateIndexer swarm

---

### Getting started (server/tracker)

First, check out the [example docker-compose-stack.yml file](docker-compose-stack.yml), then follow the guides for each component.

The [privateindexer-server guide](guides/SERVER.md) will show how to set up the 

You can then move onto the [privateindexer-tracker guide](guides/TRACKER.md) to finish up.

The guides assume you will be using the provided MySQL and Redis container configs in Docker.

---

### Distribute your swarm (client)

Direct your friends to check out the [example docker-compose-client.yml file](docker-compose-client.yml)

They can then follow the instructions in the [privateindexer-client guide](guides/CLIENT.md) to set up 

Each client will need the following from you as the server operator:

1. Your externally accessible `server URL` (refer to your `EXTERNAL_SERVER_URL` environment variable)
2. Your externally accessible `tracker URL` (refer to your `EXTERNAL_TRACKER_URL` environment variable)
3. The user's unique API key obtained from the server's admin panel (check the bottom section of the server guide)

The client app will scan media and upload torrents to the server, then start announcing to the tracker automatically.