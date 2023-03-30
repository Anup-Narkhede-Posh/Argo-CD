# Multi Cluster Deployment

3 parts
A) Add cluster to ArgoCD
B) ApplicationSet
C) Generators


## **A) Add Remote Cluster to ArgoCD:**
Limitatatins
aws cli-v-1

current deployment jobs it may affect

```console
argocd cluster add arn:aws:eks:us-east-1:605735537041:cluster/eks-qa-122  --name eks-qa-122 --server argocd.goshd.com --kubeconfig test-config  --namespace argocd
```

## **B) ApplicationSet**


- Which problem it solves?
  - To deploy Component-1 on both Cluster-A and Cluster-B, we will need two separate application.yaml files. This is because the server addresses have different values for each cluster, and other parameters such as CPU limits may also differ. Essentially, we need to override the default values.yaml file based on the specific requirements of each cluster.

- What it it?
  - 
- How it works?
  - Generators



- ### i) Single component multiple cluster
- ### ii) Multiple component multiple cluster
  - Local helm Chart
  - Remote helm chart local values.yaml

- ### iii) AWS Accounts
  - Single Account multi cluster
  - Multiple Account multi cluster

Conclusion:

- Single AWS Account
  - `1 account`, `1 component`, `n clusters`
  - `1 account`, `n components`, `n clusters`

- Multiple AWS Account
  - `n accounts`, `n components`, `n clusters`

Repo Structure of Multi AWS Account

## Architecture Diagram:

`draw.io image here`

## Repository Structure

```
myrepo
    ├── argocd
    │   ├── applications
    │   │   ├── Chart.yaml
    │   │   ├── templates
    │   │   │   ├── component-1-application.yaml
    │   │   │   └── component-2-application.yaml
    │   │   └── values.yaml
    │   └── parameters
    │       ├── non-prod
    │       │   ├── ci-cluster
    │       │   │   └── values.yaml
    │       │   ├── qa-cluster
    │       │   │   └── values.yaml
    │       │   └── stage-cluster
    │       │       └── values.yaml
    │       └── prod
    │           ├── prod-cluster-1
    │           └── prod-cluster-2
    └── components
        ├── component-1
        │   ├── Chart.yaml
        │   ├── templates
        │   │   ├── _helpers.tpl
        │   │   └── deployments.yaml
        │   └── values.yaml
        └── component-2
            ├── This_Is_Remote_Chart_Local_Values_HELM
            └── values.yaml

```
