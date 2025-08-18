# Getting Started with TrySpace Lab

A short quick-start to get a running TrySpace Lab environment.
For detailed reference see the linked pages (Installation, Configuration, Operations, Data & Cleanup, Troubleshooting).

## Walkthrough
* Ensure Docker, Docker Compose (v2+), Make and Git are installed (see `installation.md`).
* Clone and initialize submodules:

```bash
git clone https://github.com/TrySpaceOrg/tryspace-lab.git
cd tryspace-lab
git submodule update --init --recursive
```

* Prepare environment and build:

```bash
make
```

* Start the lab:

```bash
make start
```

* Access services (examples):

- GSW (YAMCS): http://localhost:8090
- Attach to consoles: `docker attach tryspace-server`

* Stop (preserves data):

```bash
make stop
```

## Where to go next
- Installation details and OS-specific notes: [Installation](installation.md)
- Configuration, orchestrator, and templates: [Configuration](configuration.md)
- Daily operations (start/stop/logs): [Operations](operations.md)
- Data, volume cleanup, and safe-reset instructions: [Data & Cleanup](data-and-cleanup.md)
- Troubleshooting and FAQ: [Troubleshooting](troubleshooting.md)
