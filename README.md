# BridgeDB - SURF Research Cloud Component

Ansible playbook component for deploying [BridgeDB](https://www.bridgedb.org/) identifier mapping service on SURF Research Cloud.

## What it does

1. Installs Docker
2. Pulls and runs the `bigcatum/bridgedb` container
3. Configures SURF's nginx as reverse proxy with OAuth2 authentication

## Catalog item setup

| Order | Component          | Purpose                          |
|-------|--------------------|----------------------------------|
| 1     | SRC-OS             | Base OS configuration            |
| 2     | SRC-CO             | Core SURF component              |
| 3     | SRC-Nginx          | Nginx with Let's Encrypt SSL     |
| 4     | SRC-External       | Required for non-SURF components |
| 5     | **This component** | BridgeDB setup                   |

### Component registration

- **Script type**: Ansible Playbook
- **Repository URL**: `https://github.com/<your-org>/bridgedb-src-component.git`
- **Script path**: `playbooks/bridgedb.yml`
- **Tag/Branch**: `main`

## Parameters

| Key                      | Default              | Description                    |
|--------------------------|----------------------|--------------------------------|
| `bridgedb_image`         | `bigcatum/bridgedb`  | Docker image                   |
| `bridgedb_tag`           | `latest`             | Docker image tag               |
| `bridgedb_port`          | `8183`               | Host port for BridgeDB         |
| `bridgedb_container_name`| `BRIDGEDB`           | Docker container name          |

## Architecture

```
Browser → SURF nginx (443/SSL + OAuth2) → BridgeDB container (8183)
```

## Directory structure

```
bridgedb-src-component/
├── README.md
└── playbooks/
    ├── bridgedb.yml                        # Entry point (script path for SURF)
    └── roles/
        └── bridgedb/
            ├── defaults/main.yml           # Default variables
            ├── handlers/main.yml           # Nginx reload handler
            ├── tasks/main.yml              # Docker + container + nginx proxy
            └── templates/
                └── nginx-bridgedb.conf.j2  # Reverse proxy config
```
