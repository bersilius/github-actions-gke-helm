# github-actions-gke-helm

This repository contains a Dockerfile for building a Docker image
that can be used in a GitHub Action workflow as a container to be
able to deliver your software to a GKE based Kubernetes cluster
via `gcloud`, `kubectl` and `helm` CLI tools.

## How it works

The container has `gcloud`, `kubectl` and `helm` installed so you can
use `gcloud` with a Google Cloud service account to configure `kubectl`,
so can use `helm` to have your software deployed, upgraded, etc.

NOTE: `gcloud` and `kubectl` may not be necessary to deliver a package,
since `helm` can be sufficient with proper configuration, but once you
run into issues you might want to have these CLI tools in your toolkit.

## Use it in your workflow

This image is also published to Docker Hub, so it can be pulled and
utilized in a workflow via using `container:` configuration, see below.

```yaml
name: deploy

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    container: 
      image: bersilius/github-actions-gke-helm:latest
    steps:
      - name: run_custom_gcloud_command
        run: gcloud container clusters get-credentials my-cluster

      - name: run_custom_kubectl_command
        run: kubectl apply -f some_manifest.yaml

      - name: run_custom_helm_command
        run: helm ls --tls
```

## ToDo

- parameterize Dockerfile
- test Helm 3 beta

## References

- [GKE documentation](https://cloud.google.com/kubernetes-engine/docs/quickstart)
- [Helm documentation](https://helm.sh/)
- [GitHub Actions documentation](https://help.github.com/en/categories/automating-your-workflow-with-github-actions)
