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

| Link                                                            | Description           |
| --------------------------------------------------------------- | --------------------- |
| [Repo Link](https://github.com/ansnoussi/gitops-example-app)    | Example GitOps App    |
| [Repo Link](https://github.com/ansnoussi/gitops-example-deploy) | Example GitOps Deploy |
| [Repo Link](https://github.com/ansnoussi/gitops-example-infra/) | Example GitOps Infra  |

## Provision Cluster (GCP)

#### Prerequisites

Make sure to have :

- gcloud clie installed locally and authenticated to your gcp account.
  - authenticate with : `gcloud auth login`
  - update components just in case : `gcloud components update`
- have kubectl installed
- have Terraform installed

#### Steps

1. clone the [Infra Repo](https://github.com/ansnoussi/gitops-example-infra/) into your local machine.
2. checkout to the gcp branch : `git checkout gcp`
3. create a `terrafrom.tfvars` containing the required vqriables:
   - `cluster_name` : Name you give your cluster
   - `linux_admin_password` : Password for the hosts in your cluster
   - `gcp_project_name` : The ID of your Google Cloud project
   - `gcp_project_region` : The region in which the cluster should be located
   - `node_locations` : Zones in which nodes should be placed
   - `cluster_cp_location` : Zone for control plane
4. Run `terraform init`
5. Run `terraform plan -var-file="terrafrom.tfvars"`
6. Run `terraform apply -var-file="terrafrom.tfvars"`
7. Get kubectl credentials from Google

```
gcloud container clusters get-credentials <CLUSTER NAME> --zone <CLUSTER CP LOCATION>
```

8. Check you have access by running : `kubectl cluster-info`
9. After finishing withe the project : `terraform destroy` to delete the k8s cluster
