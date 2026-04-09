# Setup Guide

This repository documents my home server stack, but it is not a one-command installer.

Use this guide to recreate the same style of setup on a fresh Ubuntu server.

## What You Need

- An Ubuntu server
- A user with `sudo`
- SSH access to the server
- A Tailscale account
- Basic comfort editing config files and running Docker containers

## Stack Covered Here

- Docker
- Tailscale
- UFW
- Nextcloud
- Immich
- Navidrome
- Gitea
- Portainer

## 1. Update The Server

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg ufw
```

## 2. Install Docker

Install Docker Engine and the Compose plugin using Docker's official repository:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Enable Docker and let your user run Docker commands without `sudo`:

```bash
sudo systemctl enable --now docker
sudo usermod -aG docker "$USER"
newgrp docker
```

Verify:

```bash
docker version
docker compose version
```

## 3. Install Tailscale

This is the main remote access path used by this setup.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Then verify:

```bash
tailscale status
tailscale ip -4
```

If you want HTTPS on your tailnet domain later, you can also configure Tailscale Serve after your apps are running.

## 4. Set Up Basic Firewall Rules

This repo assumes `ufw` is enabled and inbound traffic is restricted.

At minimum:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```

If you only plan to access services over Tailscale, keep public ports to a minimum.

## 5. Create The Deployment Layout

This repo documents each app as its own deployment directory under `/home/josh`.

If your username is different, replace `josh` with your own username.

```bash
mkdir -p /home/josh/nextcloud
mkdir -p /home/josh/immich-app
mkdir -p /home/josh/navidrome
mkdir -p /home/josh/gitea
```

## 6. Deploy Each Service

This repository currently does not include the actual Compose files, so the easiest path is to follow each project's official Docker guide and place the files in the directories above.

### Nextcloud

- Deployment path: `/home/josh/nextcloud`
- Official docs: <https://github.com/nextcloud/docker>
- Expected pieces:
  - Nextcloud app container
  - MariaDB container
  - Persistent `data` and `db` directories

After adding your compose file:

```bash
cd /home/josh/nextcloud
docker compose up -d
```

### Immich

- Deployment path: `/home/josh/immich-app`
- Official docs: <https://immich.app/docs/install/docker-compose>
- Expected pieces:
  - Immich server
  - Postgres
  - Valkey
  - `.env`, `library`, and database storage

After adding your compose file:

```bash
cd /home/josh/immich-app
docker compose up -d
```

### Navidrome

- Deployment path: `/home/josh/navidrome`
- Official docs: <https://www.navidrome.org/docs/installation/docker/>
- Expected pieces:
  - Navidrome container
  - Music library mount
  - Persistent app data directory

After adding your compose file:

```bash
cd /home/josh/navidrome
docker compose up -d
```

### Gitea

- Deployment path: `/home/josh/gitea`
- Official docs: <https://docs.gitea.com/installation/install-with-docker>
- Expected pieces:
  - Gitea container
  - Persistent `data` and `config`
  - Web port and SSH port mapping

After adding your compose file:

```bash
cd /home/josh/gitea
docker compose up -d
```

### Portainer

- Official docs: <https://docs.portainer.io/start/install-ce/server/docker/linux>
- This repo documents Portainer as a standalone Docker container rather than a separate compose project.

Example:

```bash
docker volume create portainer_data
docker run -d \
  --name portainer \
  --restart=always \
  -p 9443:9443 \
  -p 8000:8000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:sts
```

## 7. Verify The Stack

Check that everything is running:

```bash
docker ps
```

Useful checks:

- `tailscale status`
- `sudo ufw status`
- `docker logs <container-name>`

Ports documented in this repo:

- `22/tcp` for SSH
- `8080/tcp` for Nextcloud
- `2283/tcp` for Immich
- `4533/tcp` for Navidrome
- `3000/tcp` for Gitea web
- `2222/tcp` for Gitea SSH
- `8000/tcp` and `9443/tcp` for Portainer

## 8. Optional: Tailscale HTTPS

The README notes that this setup uses Tailscale Serve for HTTPS on the tailnet domain.

Once your services are working locally, you can add Tailscale Serve in front of one of them. Example:

```bash
sudo tailscale serve --bg http://127.0.0.1:8080
```

Adjust the local target to whichever app you want to expose through your tailnet.

## Notes

- This guide is meant to recreate the structure of the setup documented in this repo.
- It does not include backups, reverse proxy automation, or production hardening.
- If you want this repo to become fully reproducible, the next step would be adding the actual Compose files for each service.
