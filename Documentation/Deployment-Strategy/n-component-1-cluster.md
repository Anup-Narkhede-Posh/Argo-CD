# Deployment of Multiple Components on Single Cluster using ArgoCD

Effortlessly deploy and manage multiple Kubernetes applications on a single cluster with the power of ArgoCD.


## Architecture Diagram:

`draw.io image here`

# **A) App of Apps Pattern**
## Repository Structure

```
myrepo
    ├── app-of-apps
    │   └── app-of-apps.yaml
    ├── argocd
    │   ├── component-1-application.yaml
    │   └── component-2-application.yaml
    └── components
        ├── component-1
        │   ├── deployment.yaml
        │   └── service.yaml
        └── component-2
            ├── Chart.yaml
            ├── templates
            │   ├── _helpers.tpl
            │   └── deployments.yaml
            └── values.yaml

```

- `app-of-apps.yaml` file in the `app-of-apps` directory reference `path` to the `argocd` directory.
- The `argocd` directory contains YAML files that define individual ArgoCD application defination file (`component-1-applicatin.yaml` and `component-2-applicatin.yaml`)
- The `component-1-application.yaml` file in the `argocd` directory references the `path` to the `components/component-1` directory, where the deployment and service manifests for `component-1` are located.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/github-username/my-repo.git
    targetRevision: HEAD
    path: argocd/
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
```
The `app-of-apps` ArgoCD application deploys two applications: `component-1` and `component-2`. `component-1` has its deployment and service **manifests** located in the `components/component-1` directory, while `component-2` is defined using **Helm charts** in the `components/component-2` directory.

![Alt text](https://argo-cd.readthedocs.io/en/stable/assets/application-of-applications.png)

# `**insert overall argocdUI image here**`

## **B) Using Helm Templating**

## Repository Structure

```
myrepo
    ├── argocd
    │   └── applications
    │       ├── Chart.yaml
    │       ├── templates
    │       │   ├── component-1-application.yaml
    │       │   └── component-2-application.yaml
    │       └── values.yaml
    └── components
        ├── component-1
        │   ├── deployment.yaml
        │   └── service.yaml
        └── component-2
            ├── Chart.yaml
            ├── templates
            │   ├── _helpers.tpl
            │   └── deployments.yaml
            └── values.yaml

```

In this repository structure, Helm is used to package and deploy the application components in a consistent and repeatable manner.

clone git repo on cluster & change directory to argocd.
Use following command to deploy applications to argocd.

```console
helm3 install applications ./applications -f applications/values.yaml --kubeconfig ~/.kube/kubeconfig_eks-sre-122kubeconfig_eks-sre-122
```

