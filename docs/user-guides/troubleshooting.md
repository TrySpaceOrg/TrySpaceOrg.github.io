# Troubleshooting & FAQ

Short troubleshooting checklist and common issues.

## Checklist
* Are containers running? `docker ps`
* Are logs showing errors? `docker compose -f ./cfg/lab-compose.yml logs --tail 200`
* Are ports free? `ss -ltnp | grep 8090`
* Are volumes filling up? `docker system df --volumes`

## Common issues
* Data keeps growing across runs
  * Reason: Named Docker volumes persist. To reset, run `docker compose -f ./cfg/lab-compose.yml down --volumes`.

* Permission errors on created files
  * Fix: Run `make env` to populate UID/GID in `cfg/.env` and restart containers, or adjust ownership on host files.

* Port conflicts (e.g., 8090)
  * Fix: Stop the conflicting service or modify `cfg/lab-compose.yml` to use a different host port.

* Container crashes on startup
  * Inspect logs and raise an issue with the output if you need help. Useful commands:

```bash
# show recent logs for a service
docker compose -f ./cfg/lab-compose.yml logs --tail 200 tryspace-server
# inspect container exit code
docker inspect --format='{{.State.ExitCode}}' tryspace-server
```

## When to open an issue
* Reproducible crashes with logs and steps to reproduce
* Misbehavior in orchestrator merges (include `cfg/build.yaml` snapshot)
* Unexpected data corruption in persistent volumes

## Reporting checklist for issues
* `git rev-parse --abbrev-ref HEAD` (branch)
* `git rev-parse --short HEAD` (commit)
* `docker compose -f ./cfg/lab-compose.yml ps` output
* Relevant logs (attach `docker compose -f ./cfg/lab-compose.yml logs --tail 500`)
* `cfg/build.yaml` (after running orchestrator)
* `cfg/active.yaml`
