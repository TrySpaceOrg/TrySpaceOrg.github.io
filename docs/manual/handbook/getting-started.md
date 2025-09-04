# Getting Started with TrySpace Lab

A short quick-start to get a up and running with TrySpace Lab.

## Walkthrough
* Ensure Docker, Docker Compose (v2+), Make and Git are installed (see [Installation](installation.md)).
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
