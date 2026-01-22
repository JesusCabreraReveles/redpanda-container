# Redpanda Container Setup

This project contains the configuration needed to spin up a Redpanda node inside a Docker container, optimized for local development and automated deployment.

## Features

- SASL/SCRAM authentication enabled by default.
- Explicit topic creation during bootstrap (auto-create disabled).
- Configuration for Admin users (`superuser`) and application users (`producer`/`consumer`).
- ACLs configured to restrict access to topics.
- Support for VS Code Dev Containers.
- Automated deployment with GitHub Actions.

## Prerequisites

- Docker and Docker Compose installed.
- External network and volume created.

## How to run locally

Ensure the network and volume exist:

```bash
docker network create siscom-network
docker volume create redpanda_data
```

Create your `.env` file from the example:

```bash
cp .env.example .env
```

(Optional) Edit `.env` to change default users and passwords.

Build and start:

```bash
docker-compose up -d --build
```

The setup runs automatically! You can monitor progress with:

```bash
docker logs -f redpanda-0
```

### Useful Commands (RPK)

**View cluster info:**

```bash
docker exec -it redpanda-0 rpk cluster info \
  -X user=superuser \
  -X pass=secretpassword \
  -X sasl.mechanism=SCRAM-SHA-256
```

**List topics:**

```bash
docker exec -it redpanda-0 rpk topic list \
  -X user=superuser \
  -X pass=secretpassword \
  -X sasl.mechanism=SCRAM-SHA-256
```

## Docker Hub
