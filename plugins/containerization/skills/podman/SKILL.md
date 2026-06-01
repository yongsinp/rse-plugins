---
name: podman
description: Use when the user asks about Podman, rootless containers, Quadlet/systemd units, Podman Compose, or migrating Docker workflows to Podman. Creates and manages rootless Podman containers, maps Docker commands, configures podman run/build/compose flows, and sets up systemd-managed services.
metadata:
  references:
    - references/rootless-containers.md
    - references/docker-compatibility.md
    - references/operations-and-troubleshooting.md
  assets:
    - assets/podman-compose-example.yml
    - assets/quadlet-example.container
---

# Podman

## Primary Actions

- Run and manage rootless Podman containers.
- Translate Docker workflows to Podman equivalents.
- Configure Podman Compose for multi-service stacks.
- Create systemd-managed services using Quadlet.
- Troubleshoot rootless, socket, and service lifecycle issues.

## Quick Command Map (Differences Only)

- Docker socket compatibility:

```bash
systemctl --user enable --now podman.socket
export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/podman/podman.sock
```

- Pod-specific operations (Podman-specific):

```bash
podman pod create --name app-pod
```

- Quadlet service management:

```bash
systemctl --user daemon-reload
systemctl --user enable --now my-service
```

## Validation Workflow

1. Verify rootless mode and namespace setup.
2. Run a basic rootless container successfully.
3. For compose, validate config before `up`.
4. For Quadlet, verify systemd unit status and logs.
5. If failures occur, fix and re-run checks.

## Templates

- [assets/podman-compose-example.yml](assets/podman-compose-example.yml)
- [assets/quadlet-example.container](assets/quadlet-example.container)

## Deep References

- Rootless architecture, networking, volume permissions:
  [references/rootless-containers.md](references/rootless-containers.md)
- Docker compatibility details and known differences:
  [references/docker-compatibility.md](references/docker-compatibility.md)
- Validation and troubleshooting playbook:
  [references/operations-and-troubleshooting.md](references/operations-and-troubleshooting.md)
