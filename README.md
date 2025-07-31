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
<table>
    <tr>
        <td></td>
        <td>URL</td>
        <td>Comment</td>
    </tr>
    <tr>
        <td></td>
        <td>[Arr Stack](https://trash-guides.info/)</td>
        <td>Standard PVR/Downloader stack</td>
    </tr>
    <tr>
        <td></td>
        <td>[AudioBookshelf ✅](https://www.audiobookshelf.org/)</td>
        <td>Installed as part of the KubeCraft Homelab course.</td>
    </tr>
    <tr>
        <td></td>
        <td>[Jellyfin ✅](https://jellyfin.org/docs/general/installation/container)</td>
        <td>Media server</td>
    </tr>
    <tr>
        <td></td>
        <td>[Jellyseer](https://docs.jellyfineerr.dev/getting-started/docker?docker-methods=docker-compose)</td>
        <td>Jellyfin request manager</td>
    </tr>
    <tr>
        <td></td>
        <td>[Home Assistant](https://www.home-assistant.io/)</td>
        <td>Smart home management</td>
    </tr>
    <tr>
        <td></td>
        <td>[Homepage](https://gethomepage.dev/installation/k8s/) Or [Glances](https://github.com/glanceapp/glance/) — TBD</td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>[Linkding ✅](https://linkding.link/)</td>
        <td>Bookmark manager</td>
    </tr>
    <tr>
        <td></td>
        <td>[Mealie ✅](https://docs.mealie.io/)</td>
        <td>Recipe manager</td>
    </tr>
    <tr>
        <td></td>
        <td>[n8n](https://n8n.io/)</td>
        <td>Workflow automation</td>
    </tr>
    <tr>
        <td></td>
        <td>[Paperless-ngx](https://docs.paperless-ngx.com/)</td>
        <td>Document management</td>
    </tr>
    <tr>
        <td></td>
        <td>[cloudnative-pg ✅](https://cloudnative-pg.io/)</td>
        <td>PostgreSQL with PostGIS</td>
    </tr>
    <tr>
        <td></td>
        <td>[kube-prometheus-stack ✅](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#get-helm-repository-info)</td>
        <td>Monitoring stack</td>
    </tr>
    <tr>
        <td></td>
        <td>[Traefik ✅](https://doc.traefik.io/traefik/getting-started/)</td>
        <td>I initially disliked using Traefik with Docker Compose due to overly verbose files, which was likely due to misconfiguration. Kubernetes seems to offer a much cleaner setup.</td>
    </tr>
    <tr>
        <td></td>
        <td>[Wallos ✅](https://github.com/ellite/Wallos)</td>
        <td>Subscriptions Dashboard</td>
    </tr>
    <tr>
        <td></td>
        <td>[Watchtower ✅](https://github.com/containrrr/watchtower)</td>
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
