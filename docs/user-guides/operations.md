# Operations

Standard operations and common commands for running TrySpace Lab.

## Starting the lab
Foreground (useful for development and observing operations):

```bash
make start
```

## Attaching and logs
Attach to a running container:

```bash
docker attach tryspace-server
```

Follow logs for all services:

```bash
docker compose -f ./cfg/lab-compose.yml logs -f
```

Follow logs for a single service:

```bash
docker compose -f ./cfg/lab-compose.yml logs -f tryspace-server
```

## Services breakdown

### tryspace-server
* Purpose: Core server process and controls the time synronization provided by Simulith across the environment
* Typical use: pause/play or change simulation speed
* Common commands:

```bash
docker attach tryspace-server
docker compose -f ./cfg/lab-compose.yml logs -f tryspace-server
```

### tryspace-director
* Purpose: Loads component simulators and 42 libraries in time step with environment via Simulith
* Typical use: inspect logs to verify component interfacing with flight software
* Common commands:

```bash
docker attach tryspace-director
docker compose -f ./cfg/lab-compose.yml logs -f tryspace-director
```

### tryspace-fsw
* Purpose: Flight Software (FSW) container that runs the core Flight System software with component applications.
* Typical use: check FSW startup, telemetry output, and interactions
* Common commands:

```bash
docker attach tryspace-fsw
docker compose -f ./cfg/lab-compose.yml logs -f tryspace-fsw
```

### tryspace-gsw
* Purpose: Ground Software (GSW) container running YAMCS (ground station interface and telemetry archive).
* Typical use: open the web UI, examine telemetry, and export or inspect persistent data.
* Ports: 8090 -> 8090 (host) for the YAMCS web UI (confirm in `cfg/lab-compose.yml`).
* Volumes:
  * `gsw-data` — persistent YAMCS data (preserved across `docker compose down` unless `--volumes` is used).
* Healthcheck: Compose defines an HTTP healthcheck against `http://localhost:8090` (see `cfg/lab-compose.yml`).
* Common commands:

```bash
# Open UI in a browser
firefox http://localhost:8090

docker compose -f ./cfg/lab-compose.yml logs -f tryspace-gsw
# inspect persistent data
docker volume inspect gsw-data
```

## Stopping
Stop containers and remove networks (preserves volumes):

```bash
make stop
```

Stop and remove containers, network, and named volumes (clean state):

```bash
make clean
```

## Uninstalling
To uninstall all pieces of the tryspace environment:

```bash
make uninstall
```
