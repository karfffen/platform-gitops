# platform-gitops

This repository is a **read-only mirror**. Do not edit it here: any change is overwritten by the next sync.

The source is the `gitops/` folder in [karfffen/infra](https://github.com/karfffen/infra). Changes are made there by pull request, and `gitops-sync.yml` publishes them here after merge (ADR-0014).

Flux on the AKS clusters reads this repository anonymously.

## Layout

- `clusters/blue`, `clusters/green`: entry points. Each defines the Flux Kustomizations `platform` and `apps` (`apps` depends on `platform`; interval 10m; prune off).
- `platform/`: cluster add-ons shared by both clusters (namespaces, network policies, quotas): `frontend` and `backend`, each denied by default.
- `apps/`: one folder per workload. Now: `hello-world`, Microsoft's sample page in `frontend`, routed by the ingress.
- `.kube-version`: the passive cluster's Kubernetes minor. CI checks manifests against it.

Everything here is public: never add secrets.
