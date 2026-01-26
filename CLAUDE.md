# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Docker container running [chrony](https://chrony-project.org/) NTP server on Alpine Linux. Provides Network Time Protocol (NTP) services to other computers on the network.

## Build and Run Commands

```bash
# Build the Docker image locally
./build.sh

# Run the container locally
./run.sh

# Build multi-architecture image (requires docker buildx)
./build-multiarch.sh           # dry run (no push)
./build-multiarch.sh --go-time # build and push to registry

# Using docker-compose
docker compose up -d ntp
docker compose logs ntp
```

## Configuration

All configuration is done via environment variables, defined in the `vars` file for local scripts:

- `NTP_SERVERS` - Comma-delimited list of NTP servers (default: `time.cloudflare.com`)
- `ENABLE_NTS` - Enable Network Time Security when `true`
- `NOCLIENTLOG` - Disable client access logging when `true`
- `LOG_LEVEL` - Chrony log level: 0 (info), 1 (warning), 2 (non-fatal error), 3 (fatal error)
- `TZ` - Timezone (default: UTC)

## CI/CD Pipeline

GitHub Actions workflow (`.github/workflows/build.yml`) runs on pushes to `main`, `develop`, `feature/**` branches and semver tags:

1. **Lint** - Dockerfile linting with hadolint, changelog validation
2. **Build** - Multi-arch Docker image build (amd64, arm64), pushes to GHCR on tags
3. **Scan** - Trivy security scanning with SARIF upload to GitHub Security
4. **Release** - Creates GitHub release from CHANGELOG.md (tags only)

## Architecture

```
Dockerfile           # Alpine-based image with chrony
assets/startup.sh    # Entrypoint: generates chrony.conf from env vars, starts chronyd
vars                 # Local development configuration variables
docker-compose.yml   # Compose file for local development
```

The startup script dynamically generates `/etc/chrony/chrony.conf` at container start based on environment variables, then runs chronyd with `-x` flag (does not control host system clock).

## Testing the Container

```bash
# Check NTP sync status
docker exec ntp chronyc tracking

# View configured NTP sources
docker exec ntp chronyc sources

# Query from external host
ntpdate -q <DOCKER_HOST_IP>
```
