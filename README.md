# Redpanda Container Setup

This project contains the configuration needed to spin up a Redpanda node inside a Docker container, optimized for local development and automated deployment.

## Features

- SASL/SCRAM authentication enabled by default.
- Explicit topic creation during bootstrap (auto-create disabled).
- Configuration for Admin users (`super_user`) and application users (`producer_user`/`consumer_user`).
- ACLs configured to restrict access to the `test` topic.
- Support for VS Code Dev Containers.
- Automated deployment with GitHub Actions.

## Credentials and Topics

By default, the following users and topics are created:

| User | Password | Role |
| :--- | :--- | :--- |
| `super_user` | `secretpassword` | Administrator (all topics) |
| `producer_user` | `producerpassword` | Producer for `test` topic |
| `consumer_user` | `consumerpassword` | Consumer for `test` topic |

- **Topic:** `test` (1 partition, 1 replica)

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
  -X user=super_user \
  -X pass=secretpassword \
  -X sasl.mechanism=SCRAM-SHA-256
```

**List topics:**

```bash
docker exec -it redpanda-0 rpk topic list \
  -X user=super_user \
  -X pass=secretpassword \
  -X sasl.mechanism=SCRAM-SHA-256
```

## Docker Hub

jesuscabrera1984/redpanda-sasl-bootstrap
