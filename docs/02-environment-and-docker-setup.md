# 2. Environment and Docker Setup

## Host environment

The lab was built on a Windows computer using:

- Windows host
- WSL 2
- Ubuntu
- Docker Desktop / Docker Compose
- Project workspace under the Ubuntu home directory: `~/soc-lab`

The Ubuntu shell username used in the lab was `cybreach`.

## Verify the Linux environment

Open Ubuntu/WSL and run:

```bash
whoami
pwd
ls -la ~
```

The project workspace is:

```bash
mkdir -p ~/soc-lab
cd ~/soc-lab
```

## Verify Docker

Run:

```bash
docker --version
docker compose version
docker ps
docker run hello-world
```

The `hello-world` container is a basic check that Docker can run a container.

If Docker commands require elevated permissions, check that the current user has the appropriate Docker group membership and restart the WSL session after changing group membership. Do not blindly change permissions on Docker socket files.

## Project folders

The main folders used in this lab include:

```text
~/soc-lab/
├── wazuh/
│   └── wazuh-docker/
│       └── single-node/
└── Shuffle/
```

The Wazuh Docker deployment was kept under `~/soc-lab/wazuh/wazuh-docker/single-node`. Shuffle was kept under `~/soc-lab/Shuffle`.

## Start and inspect services

Run Compose commands from the directory containing the relevant Compose file.

```bash
docker compose up -d
docker compose ps
docker compose logs --tail=100
```

To inspect a specific container:

```bash
docker ps
docker logs --tail=100 <container_name>
```

Use the actual service/container names shown by `docker ps`; do not assume names from another installation.

## Notes

The exact Compose files and environment variables can differ by version. Keep the original deployment files in your private working environment and publish only sanitised examples. Never commit credentials, API keys, bot tokens, private keys, or certificate material.
