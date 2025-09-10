# Installation

This page lists prerequisites and OS-specific tips for installing TrySpace Lab dependencies.

## Prerequisites
* Docker Engine
* Docker Compose v2 (CLI plugin) or newer
* Make
* Git

## Windows notes
For optimal performance on Windows we'd recommend WSL2 and Docker Engine installed.
You can simply follow your specific Linux OS install for docker and docker compose for WSL2.

If you prefer to work in a Virtual Machine, ensure you have Hyper-V disabled for VirtualBox (to avoid the green turtle) by using the official Windows DG Readiness Tool which completely disables Hyper-V and enables the other settings required.

## Linux / WSL notes
Be sure to add your user to the `docker` group!

## Verifying installation
```bash
docker --version
docker compose version
make --version
git --version
```

<img src="/assets/manual/installation/Screenshot 2025-09-10 083345.png" alt="Screenshot 2025-09-10 083345" class="center" />
