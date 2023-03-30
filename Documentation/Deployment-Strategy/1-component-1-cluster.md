# Deployment of Single Component on Single Cluster using ArgoCD


## Architecture Diagram:
![Alt text](https://github.com/Anup-Narkhede-Posh/Argo-CD/blob/main/Documentation/Deployment-Strategy/Images/application.jpeg)
## Repository Structure

```
myrepo
    ├── argocd
    │   └── component-1-application.yaml
    └── components
        └── component-1
            ├── deployment.yaml
            └── service.yaml
```


- `argocd`: a directory containing ArgoCD application definition files.
  - `component-1-application.yaml`: a YAML file that defines an ArgoCD Application object for the component-1.
- `components`: a directory that holds various components of the application, which can be in the form of plain Kubernetes manifests, Helm charts, Kustomize components, or JSONet
  - `component-1`: a directory containing Kubernetes manifest files for a specific component of the application named `component-1`.
    - `deployment.yaml`: a YAML file that defines a Kubernetes Deployment object for `component-1`.
    - `service.yaml`: a YAML file that defines a Kubernetes Service object for `component-1`.

## ArgoCD Application Configuration

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: component-1
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/github-username/my-repo.git
    path: components/component-1/
  destination:
    server: https://kubernetes.default.svc
    namespace: component-1
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
```

`application.jpeg`


The YAML code above represents the configuration for an ArgoCD Application object.

- `apiVersion:` specifies the ArgoCD API version used in the file, which is **argoproj.io/v1alpha1**.
- `kind:` specifies the Kubernetes kind of the object, which is Application **"CRD"**.
- `metadata:` contains metadata for the object, such as the name and namespace.
  - `name:` specifies the name of the ArgoCD Application object, which is **component-1**.
  - `namespace:` specifies the Kubernetes namespace where the object will be created, which is **argocd**.
- `spec:` contains the specification of the ArgoCD Application object.
  - `project:` specifies the name of the ArgoCD project to which the application belongs. In this case, the project is named **default**. (Logical grouping of applications).
  - `source:` specifies the source of the application's manifests.
    - `repoURL:` specifies the URL of the Git repository where the manifests are stored.
    - `path:` specifies the directory path within the repository where the manifests for this application are located. In this case, the path is **components/component-1/**.
  - `destination:` specifies the destination cluster where the application's manifests will be deployed.
    - `server:` specifies the Kubernetes API server URL where the manifests will be deployed. In this case, the URL is **https://kubernetes.default.svc**.
    - `namespace:` specifies the Kubernetes namespace where the manifests will be deployed. In this case, the namespace is **component-1**.
  - `syncPolicy:` specifies the synchronization policy for the application.
    - `syncOptions:` specifies additional options for the synchronization process. In this case, the option 
      - `CreateNamespace` is set to **true**, which will create the namespace **component-1** if it doesn't exist when the application is deployed.




To deploy and import the `component-1` application in ArgoCD using the `kubectl apply` command, apply the configuration by running 
```console
kubectl apply -f component-1-application.yaml
```
 in the directory where the `component-1-application.yaml` file is located.
