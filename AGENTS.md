# AGENTS.md

## Cursor Cloud specific instructions

### Overview

This is a GitHub Actions "Hello World" tutorial repository. The core product is a Docker-based GitHub Action in `action-a/` that prints a greeting, exercised by a workflow in `.github/workflows/main.yml`.

### Prerequisites (installed in the update script)

- **Docker** — required to build and run the action container (`action-a/Dockerfile`)
- **act** (`nektos/act`) — runs GitHub Actions workflows locally without pushing to GitHub

### Running the action locally

**Direct Docker test:**
```
cd action-a && docker build -t hello-action . && docker run --rm hello-action "Mona"
```

**Full workflow test with `act`:**
```
act push --container-architecture linux/amd64
```

### Gotchas

- Docker runs inside a nested container (Firecracker VM). The daemon must use `fuse-overlayfs` storage driver and `iptables-legacy`. These are configured during environment setup.
- `act` requires a pre-configured `~/.config/act/actrc` file to avoid an interactive image-size picker. The update script creates this with the medium-size runner image (`catthehacker/ubuntu:act-latest`).
- The `mesg: ttyname failed` message in Docker output is harmless and expected in non-TTY environments.
- The first run of `act` pulls the runner image (~500MB) which takes a minute; subsequent runs use the cached image.
