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
<table style="width:100%">
    <tr>
        <th style="width:25%"></td>
        <th style="width:25%">URL</td>
        <th style="width:50%">Comment</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/sonarr-radarr.svg"></td>
        <td><a href="https://trash-guides.info">Arr Stack</a></td>
        <td>Standard PVR/Downloader stack</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/audiobookshelf.svg"></td>
        <td><a href="https://www.audiobookshelf.org/">AudioBookshelf ✅</a></td>
        <td>Installed as part of the KubeCraft Homelab course.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/postgresql.svg"></td>
        <td><a href="https://cloudnative-pg.io/">CloudNativePG ✅</a></td>
        <td>PostgreSQL with PostGIS</td>
    </tr>
    <tr>
        <td></td>
        <td><a href="https://gethomepage.dev/installation/k8s/">Homepage</a> Or <a href="https://github.com/glanceapp/glance/">Glances</a></td>
        <td>TBD</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/home-assistant.svg"></td>
        <td><a href="https://www.home-assistant.io/">Home Assistant</a></td>
        <td>Smart home management</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/jellyfin.svg"></td>
        <td><a href="https://jellyfin.org/docs/general/installation/container">Jellyfin ✅</a></td>
        <td>Media server</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/jellyseerr.svg"></td>
        <td><a href="https://docs.jellyfineerr.dev/getting-started/docker?docker-methods=docker-compose">Jellyseer</a></td>
        <td>Jellyfin request manager</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/grafana.svg"></td>
        <td><a href="https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#get-helm-repository-info">Kube Prometheus Stack ✅</a></td>
        <td>Monitoring stack</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/sissbruecker/linkding/refs/heads/master/assets/logo.svg"></td>
        <td><a href="https://linkding.link/">Linkding ✅</a></td>
        <td>Bookmark manager</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/mealie.svg"></td>
        <td><a href="https://docs.mealie.io/">Mealie ✅</a></td>
        <td>Recipe manager</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/n8n.svg"></td>
        <td><a href="https://n8n.io/">n8n<a/></td>
        <td>Workflow automation</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/paperless-ngx.svg"></td>
        <td><a href="https://docs.paperless-ngx.com/">Paperless-ngx</a></td>
        <td>Document management</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/traefik.svg"></td>
        <td><a href="https://doc.traefik.io/traefik/getting-started/">Traefik ✅</a></td>
        <td>I initially disliked using Traefik with Docker Compose due to overly verbose files, which was likely due to misconfiguration. Kubernetes seems to offer a much cleaner setup.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/wallos.svg"></td>
        <td><a href="https://github.com/ellite/Wallos">Wallos ✅</a></td>
        <td>Subscriptions Dashboard</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/selfhst/icons/svg/mend-renovate.svg"></td>
        <td><a href="https://github.com/containrrr/watchtower">Watchtower ✅</a></td>
        <td>Replaced by Renovate/GitOps</td>
    </tr>
</table>

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
