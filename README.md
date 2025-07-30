# Homelab

**A Work in Progress** with the goal of setting up a Kubernetes homelab, implementing GitOps practices. Part of following the Homelab course by [KubeCraft](https://www.skool.com/kubecraft), a kubernetes course by [Mischa van den Burg](https://github.com/mischavandenburg)

Currently working on converting as many of my docker compose's into Kubernetes manifests. I will try to use Helm in some cases but mainly writing as much `.yaml` as possible. One of the current setbacks is to migrate the data from my docker deployments into Kubernetes.

## Hardware

I'm using a `GMKtec G3 Plus, Intel Twin Lake N150 512GB SSD 16GB RAM` running Arch Linux and I'm accessing it via SSH.

Previously I had Debian 12 (Bookworm) installed and while that was working great & stable, I wanted something that could provide me with some more up to date package-releases.

## Apps I currently run with docker compose

- [Arr stack](https://trash-guides.info/)
- [AudioBookshelf ✅](https://www.audiobookshelf.org/) - Installed as part of the KubeCraft Homelab course.
- [Jellyfin ✅](https://jellyfin.org/docs/general/installation/container) + [Jellyseer](https://docs.Jellyfineerr.dev/getting-started/docker?docker-methods=docker-compose)
- [Homessistant](https://www.home-assistant.io/)
- [Homepage](https://gethomepage.dev/installation/k8s/) or [Glances](https://github.com/glanceapp/glance/) TBD
- [Linkding ✅](https://linkding.link/) - Installed as part of the KubeCraft Homelab course.
- [Mealie ✅](https://docs.mealie.io/)
- [N8N](https://n8n.io/)
- [Paperless-ngx](https://docs.paperless-ngx.com/)
- PostgreSQL with PostGIS extension (Will be replaced with something like [cloudnative-pg](https://cloudnative-pg.io/))
- [Prometheus Community kube-prometheus-stack ✅](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#get-helm-repository-info)
- [Traefik ✅](https://doc.traefik.io/traefik/getting-started/) - ~Honestly, I really dislike using this app with docker-compose as I believe the implementation led to alot of verbose compose files. Hopefully, this was just a mistake by my end~. Bit different implementation in Kubernetes, so this seems OK for now :).
- [WallOs ✅](https://github.com/ellite/Wallos)
- [WatchTower ✅](https://github.com/containrrr/watchtower) - Assume this will be *replaced* with GitOps principles. Replaced by renovate :)

### Using Porkbun-webhook for ACME DNS01 Solver

[Porkbun](https://porkbun.com/) is not directly supported by [cert-manager](https://cert-manager.io), so it have to be used via a webhook to be able to provide a [Let's Encrypt](https://letsencrypt.org/). Using [Cloudflare](https://www.cloudflare.com/) as my domain provider would definitely make this usage easier, perhaps I will migrate in the future.

Anyways, to install the webhook one needs to clone the [Porkbun-webhook](https://github.com/mdonoughe/porkbun-webhook) repository, `cd` into the directory and install helm

```bash
helm install porkbun-webhook ./deploy/porkbun-webhook --namespace cert-manager  --set groupName=christophervestman.com
```

and make sure to configure the `issuer`, see the `infrastructure/` directory.
