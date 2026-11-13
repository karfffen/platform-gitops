# platform-gitops

This repository is a **read-only mirror**. Do not edit it here: any change is overwritten by the next sync.

The source is the `gitops/` folder in [karfffen/infra](https://github.com/karfffen/infra). Changes are made there by pull request, and `gitops-sync.yml` publishes them here after merge (ADR-0014).

Flux on the AKS clusters reads this repository anonymously.

## Layout

- `clusters/blue`, `clusters/green`: entry points. Each defines the Flux Kustomizations `platform` and `apps` (`apps` depends on `platform`; interval 10m; prune off).
- `clusters/k3s-blue`, `clusters/k3s-green`: the same entry points for the self-hosted k3s clusters (ADR-0026). `platform` reads `./platform-k3s`, which is `./platform` with the Azure Disk StorageClass switched to local-path and the snapshot class left out; `ingress` installs Traefik with App Routing's IngressClass name; `ui` installs the read-only Headlamp UI; `gitops-apps.yaml` is the source for the private app repo, which a job signs in to with a short-lived token kept outside git (ADR-0026 Amendment 2).
- `platform/`: cluster add-ons shared by both clusters (namespaces, network policies, quotas): `frontend` and `backend`, each denied by default.
- `apps/`: one folder per workload. Now: `hello-world`, Microsoft's sample page in `frontend`, routed by the ingress.
- `.kube-version`: the passive cluster's Kubernetes minor. CI checks manifests against it.

Everything here is public: never add secrets.
