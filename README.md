# Homelab

**WIP** with the goal of setting up a Kubernetes homelab, implementing GitOps practices.

Currently working on converting my docker compose into Kubernetes manifests. I will try to use Helm in some cases but mainly writing as much `.yaml` as possible.

## Hardware

I'm using a `GMKtec G3 Plus, Intel Twin Lake N150 512GB SSD 16GB RAM` running Arch Linux and I'm accessing it via SSH. Previously I had Debian 12 (bookworm) installed and while that was working just fine & stable, I wanted something that could provide me with some more up to date package-releases.

## Apps I currently run with docker compose

Note: Have to research if the apps even are wise to run in a kubernetes cluster, I belive I've read somewhere that Homessistant would be a bad example app to run in Kubernetes. Why? I don't know and I can't remember if that was true or not, but it seems like I will have to figure out that myself.

- [Arr stack](https://trash-guides.info/)
- [Jellyfin ✅](https://jellyfin.org/docs/general/installation/container) + [Jellyseer](https://docs.jellyseerr.dev/getting-started/docker?docker-methods=docker-compose)
- [Homessistant](https://www.home-assistant.io/)
- [Homepage](https://gethomepage.dev/installation/k8s/) or [Glances](https://github.com/glanceapp/glance/) TBD
- [Linkding ✅](https://linkding.link/)
- [Mealie ✅](https://docs.mealie.io/)
- [N8N](https://n8n.io/)
- [Paperless-ngx](https://docs.paperless-ngx.com/)
- PostgreSQL with PostGIS extension (Will be replaced with something like [cloudnative-pg](https://cloudnative-pg.io/))
- [Prometheus Community kube-prometheus-stack ✅](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#get-helm-repository-info)
- [Traefik](https://doc.traefik.io/traefik/getting-started/) - Honestly, I really dislike using this app. My implementation led to alot of alot of verbose compose files. Hopefully, this was just a mistake by my end.
- [WallOs](https://github.com/ellite/Wallos)
- [WatchTower](https://github.com/containrrr/watchtower) - Assume this will be *replaced* with GitOps principles
