# Homelab

**Work in Progress** — Aiming to build a Kubernetes-based homelab using GitOps practices. This setup follows the [KubeCraft](https://www.skool.com/kubecraft) course by [Mischa van den Burg](https://github.com/mischavandenburg).

I'm currently in the process of migrating my existing Docker Compose setups into Kubernetes manifests. While I intend to use Helm for some deployments, I’m primarily writing plain `.yaml` manifests to gain a deeper understanding of Kubernetes. One of the main challenges so far has been migrating persistent data (such as SQLite and PostgreSQL databases) from Docker volumes into Kubernetes-native storage.

---

## Hardware

I'm running everything on a **GMKtec G3 Plus** (Intel Twin Lake N150, 512GB SSD, 16GB RAM), with **Arch Linux** installed. Which I access via SSH.

Previously, I ran **Debian 12 (Bookworm)**, which was stable and reliable. However, I switched to Arch to benefit from more up-to-date package versions and rolling releases.

---

## Apps

My apps I currently use via Docker Compose.

[Arr stack](https://trash-guides.info/)
[AudioBookshelf ✅](https://www.audiobookshelf.org/) - Installed as part of the KubeCraft Homelab course.
[Jellyfin ✅](https://jellyfin.org/docs/general/installation/container) + [Jellyseer](https://docs.Jellyfineerr.dev/getting-started/docker?docker-methods=docker-compose)
[Homessistant](https://www.home-assistant.io/)
[Homepage](https://gethomepage.dev/installation/k8s/) or [Glances](https://github.com/glanceapp/glance/) TBD
[Linkding ✅](https://linkding.link/) - Installed as part of the KubeCraft Homelab course.
[Mealie ✅](https://docs.mealie.io/)
[N8N](https://n8n.io/)
[Paperless-ngx](https://docs.paperless-ngx.com/)
[cloudnative-pg ✅](https://cloudnative-pg.io/) - PostgreSQL (with PostGIS)
[Prometheus Community kube-prometheus-stack ✅](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#get-helm-repository-info)
[Traefik ✅](https://doc.traefik.io/traefik/getting-started/) - I initially disliked using Traefik with Docker Compose due to overly verbose files, which was likely due to misconfiguration. Kubernetes seems to offer a much cleaner setup.
[WallOs ✅](https://github.com/ellite/Wallos)
[WatchTower ✅](https://github.com/containrrr/watchtower) - ~Assume this will be *replaced* with GitOps principles.~ Replaced by renovate :)

---

### Using Porkbun-webhook for ACME DNS01 Solver

Since [Porkbun](https://porkbun.com/) is not officially supported by [cert-manager](https://cert-manager.io), I’m using a webhook-based solver to enable [Let's Encrypt](https://letsencrypt.org/) certificates via DNS-01 challenges.

If I were using [Cloudflare](https://www.cloudflare.com/) as my DNS provider, this would be much simpler — I may consider switching in the future.

To install the Porkbun webhook:

1. Clone the [porkbun-webhook](https://github.com/mdonoughe/porkbun-webhook) repository
2. Navigate into the project directory
3. Install the Helm chart:

```bash
helm install porkbun-webhook ./deploy/porkbun-webhook --namespace cert-manager  --set groupName=christophervestman.com
```

4. Then, configure your cert-manager Issuer (see the infrastructure/ directory for examples).
