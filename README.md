# homelab-cd

This is a repository for all my homelab stuff running on a baremetal K3s cluster managed by ArgoCD. For more details refer this blog I have written on medium.

📝 **Full write-up:** [Building a Multi-Node Kubernetes Cluster and Managing It with GitOps](https://medium.com/@rohanraj.madanu/building-a-multi-node-kubernetes-cluster-and-managing-it-with-gitops-ffb774c1caa3)

---

## Overview

Two physical machines connected as a k3s cluster, managed remotely from an Arch Linux workstation. NFS-based storage eliminates node dependency for stateful workloads. ArgoCD handles all deployments — any manifest pushed to this repo is automatically applied to the cluster.

---

## Stack

| Layer | Tool |
|---|---|
| Kubernetes distribution | k3s |
| GitOps / CD | ArgoCD |
| Ingress | NGINX Ingress Controller |
| Persistent storage | NFS + local-path provisioner |
| Monitoring | Prometheus + Grafana (Helm) |
| Applications | Jellyfin, Navidrome |

---

## Cluster Architecture

```
  Control Plane (Laptop #1)  ◄──►  Worker Node (Laptop #2)
            │                               │
            └──────────── NFS Server ───────┘
                          (Laptop #3)

  Managed remotely via kubectl from Arch Linux workstation
```

---

## Repository Structure

```
homelab-cd/
├── apps/
│   ├── navidrome/
│   └── jellyfin/
├── argocd/
├── monitoring/
└── storage/
```

---

## How It Works

All of my Kubernetes manifests like Deployments, Services, PVCs, Ingress rules live in this repo. ArgoCD tracks the repo and reconciles cluster state on every push. Any manual change made directly to the cluster is detected and reverted. Git is the only way to make lasting changes.

If I wanted a newer version of jellyfin I would just update the version in the manifests and it would get reflected on my cluster. The best is I could even do this remotely if I'm not at my home for some reason.

---

## Applications

- **Navidrome** — self-hosted music server, backed by NFS storage for the music library and config
- **Jellyfin** — self-hosted media server, similarly NFS-backed for media files and application state

---

## Author

**M Rohan Raj** — [linkedin.com/in/rohanraaj](https://linkedin.com/in/rohanraaj)
