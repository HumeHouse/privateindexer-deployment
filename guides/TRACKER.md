# PrivateIndexer Tracker

This is the tracker container for the HumeHouse PrivateIndexer swarm system.

- It keeps a record of all active peer connections and serves peer lists to clients for establishing new connections.

> [!IMPORTANT]  
> This container run as non-root user, make sure the permissions on your system match your configuration.

---

### Required Environment Variables

| Variable               | Description                                                                                                        | Example                         |
|------------------------|--------------------------------------------------------------------------------------------------------------------|---------------------------------|
| `REDIS_HOST`           | Redis server hostname for storing peer connections and server analytics.                                           | `privateindexer-redis`          |
| `MYSQL_HOST`           | MySQL server hostname for storing torrent metadata and maintaining user database.                                  | `privateindexer-mysql`          |

### Optional Environment Variables

| Variable                  | Default Value     | Type                     | Description                                                                                                                                  |
|---------------------------|-------------------|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| `WORKER_THREADS`          | `5`               | `INTEGER` (threads)      | Number of parallel instances of the tracker API to run at a time. This uses Uvicorn load balancing to distribute requests.                   |
| `PEER_TIMEOUT`            | `1800`            | `INTEGER` (seconds)      | Number of seconds since the last tracker announcement to consider a peer as dead and be ignored.                                             |
| `HIGH_LATECY_THRESHOLD`   | `250`             | `INTEGER` (milliseconds) | Threshold or tolerance for latency during API request processing. Any request duration over this limit will print a warning in the logs.     |
| `ANNOUNCE_INTERVAL`       | `900`             | `INTEGER` (seconds)      | The number of seconds to tell the clients to delay their next re-announce. Setting this too low can cause TONS of requests.                  |
| `ANNOUNCE_JITTER_PERCENT` | `15`              | `INTEGER` (1-100)        | Amount of jitter to add/remove from the `ANNOUNCE_INTERVAL` to help stagger the announcement requests being received from clients over time. |
| `MYSQL_PORT`              | `3306`            | `INTEGER` (port)         | Port number to use when connecting to MySQL server.                                                                                          |
| `MYSQL_USER`              | `privateindexer`  | `TEXT`                   | Username to authenticate with MySQL server.                                                                                                  |
| `MYSQL_PASSWORD`          | `privateindexer`  | `TEXT`                   | Password to use for authenticating the user to with MySQL server.                                                                            |
| `MYSQL_DB`                | `privateindexer`  | `TEXT`                   | Name of the MySQL schema or database to connect to and use for data storage.                                                                 |
| `LOG_LEVEL`               | `INFO`            | `KEYWORD` (level)        | Lowest log level to show in console. Can be `DEBUG`, `INFO`, `WARNING`, `ERROR`, or `CRITICAL` where `DEBUG` shows most amount of logs.      |
| `TZ`                      | `America/Chicago` | `TEXT` (ISO 8601)        | Change this to your desired time zone. Check online for a list of ISO 8601 time zones.                                                       |
| `UID`                     | `1000`            | `INTEGER` (user)         | User ID on the system to run the app as. Make sure this user can read and write to the `app data` directory.                                 |
| `GID`                     | `1000`            | `INTEGER` (group)        | Group ID on the system to run the app as. Make sure this group can read and write to the `app data` directory                                |

### Volumes

The tracker stores none of its own data to the disk. All peer connections are kept on Redis.

> [!TIP]
> Mount `/app/logs` somewhere on the host if you would like to have persistent log files saved.

### Port Forwarding

- The API web server runs on port 8082 inside the container to expose the announcment endpoint which clients will use to request peers or notify of state changes.
- You can map the web server port to any port on the host or none at all if you connect from within the Docker
  network, such as using a reverse proxy like NGINX.
