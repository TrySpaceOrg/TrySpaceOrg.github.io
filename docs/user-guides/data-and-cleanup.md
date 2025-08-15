# Data and Cleanup

Details about persistent data, named volumes, and safe cleanup.

## Named volumes used
- `gsw-data` - stores GSW/YAMCS persistent data (intended to survive restarts for post-processing).
- `simulith_ipc` - used for inter-process IPC between simulith components.

List volumes:

```bash
docker volume ls
```

Inspect a volume's mountpoints:

```bash
docker volume inspect gsw-data
```

## Common cleanup commands
* Stop containers and keep volumes (safe default):

```bash
make stop
```

* Clean built files, volumes, and networks:

```bash
make clean
```

## Safety notes
* `make clean` is destructive
  * Confirm backups or exports before running `down --volumes` on production or long-running datasets
