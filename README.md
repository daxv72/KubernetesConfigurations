# Kubernetes configurations

This repository holds the Kubernetes manifests for the todo project. The application code is in [KubernetesSubmissions](https://github.com/daxv72/KubernetesSubmissions). Argo CD reads only this repository.

A change to a manifest does not build an image. A change to the application code does not, by itself, change what the cluster runs. The code repository workflow copies the image tags into `base/kustomization.yaml` when `CONFIG_REPO_TOKEN` is set.

| Path | Argo CD Application | Revision | Namespace |
| --- | --- | --- | --- |
| `staging` | `staging` | branch `main` | `staging` |
| `production` | `production` | latest semver tag | `production` |

Staging sets `BROADCAST_MODE=log` and does not install `todo-webhook` or the CronJob `todo-backup`. Production keeps the webhook and the backup. The subjects are `todos.staging` and `todos.production`.

The Secret `postgres-auth` is not in this repository. Apply it in each namespace before the Pods start.
