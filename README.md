# devops-project-3

GitOps example on GKE with Terraform, k8s, and Github actions

## Overview

![Architecture Description](./overview.drawio.svg)

## Steps

**Step 0** (**Optional**) : If you have an existing k8s cluster, this steps uses Terraform to create a new cluster.
**Step 1** : There are 2 ways to trigger a release

- A developer updates the repo containing the [app repo](https://github.com/ansnoussi/gitops-example-app).
- An operator changes the deployment code in the [deployment repo](https://github.com/ansnoussi/gitops-example-deploy).

**Step 2** : Pushing or merging the change to the main branch of the app repo triggers a Docker build process.

**Step 3** : If the Docker image build is successful, it is pushed to the Docker image repository, and has a unique hash given to it as the image reference.

**Step 4** : Following the Docker image push, the deployment Git repository is updated with the new image reference.

**Step 5** : Following the Docker image push, the deployment Git repository is updated with the new image reference.

**Step 6** : Flux controller detects changes in deployment repo repository and apply them to the workload running in the cluster.

## Repositories

| Link                                                            | Description                     |
| --------------------------------------------------------------- | ------------------------------- |
| [Repo Link](https://github.com/ansnoussi/gitops-example-app)    | Example GitOps App              |
| [Repo Link](https://github.com/ansnoussi/gitops-example-deploy) | Example GitOps Deploy           |
| [Repo Link](https://github.com/ansnoussi/gitops-example-infra/) | Example GitOps Infra Repository |
