# Argo CD Installation 
Steps to install Argo CD on Kubernetes cluster.

## Prerequisites
- A running Kubernetes cluster
- The kubectl command-line tool
- Administrative privileges on Kubernetes cluster
- Helm 

## Step 1: Visit the Argo CD website
Head over to the official Argo CD website (https://argoproj.github.io/argo-cd/).

## Step 2: Create a new namespace

Create a new namespace for Argo CD in Kubernetes cluster. This namespace will isolate Argo CD and its resources from the rest of cluster.

To create a namespace, use the following command:

```console
$ kubectl create ns argocd
namespace/argocd created
$ kubectl get ns
NAME               STATUS   AGE
argocd             Active   10s
default            Active   54d
kube-node-lease    Active   54d
kube-public        Active   54d
kube-system        Active   54d
```



## Step 3: Install Argo CD

### `Way 1: Using kubectl apply`

To install Non-HA v2.6.2 ArgoCD within argocd namespace run the below command:

```console
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.6.2/manifests/install.yaml
``` 

### `Way 2: Using Helm`
a) Add the Argo CD helm repository:
```console
$ helm repo add argo https://argoproj.github.io/argo-helm
```
b) Install Argo CD using Helm:
```console
$ helm install argocd argo/argo-cd -n argocd
```

Now, Way 1 or Way 2 create all the necessary resources for Argo CD, such as deployments, services, and config maps.

```console
$ kubectl get all -n argocd

NAME                                                    READY   STATUS    RESTARTS         AGE
pod/argocd-application-controller-0                     1/1     Running   9 (5d20h ago)    33d
pod/argocd-applicationset-controller-587588cd69-g85lw   1/1     Running   9 (5d20h ago)    33d
pod/argocd-dex-server-5fbb6d7489-8m57n                  1/1     Running   9 (5d20h ago)    33d
pod/argocd-notifications-controller-7bf9b85f77-57xhf    1/1     Running   9 (5d20h ago)    33d
pod/argocd-redis-598f75bc69-5pn9l                       1/1     Running   20 (5d20h ago)   54d
pod/argocd-repo-server-75786b8cb-7h55v                  1/1     Running   9 (5d20h ago)    33d
pod/argocd-server-5dc57779d8-7qllf                      1/1     Running   9 (5d20h ago)    33d

NAME                                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.111.152.16   <none>        7000/TCP,8080/TCP            54d
service/argocd-dex-server                         ClusterIP   10.97.164.101   <none>        5556/TCP,5557/TCP,5558/TCP   54d
service/argocd-metrics                            ClusterIP   10.108.235.53   <none>        8082/TCP                     54d
service/argocd-notifications-controller-metrics   ClusterIP   10.108.206.9    <none>        9001/TCP                     54d
service/argocd-redis                              ClusterIP   10.97.10.198    <none>        6379/TCP                     54d
service/argocd-repo-server                        ClusterIP   10.107.234.9    <none>        8081/TCP,8084/TCP            54d
service/argocd-server                             ClusterIP   10.99.137.9     <none>        80/TCP,443/TCP               54d
service/argocd-server-metrics                     ClusterIP   10.103.15.175   <none>        8083/TCP                     54d

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   1/1     1            1           54d
deployment.apps/argocd-dex-server                  1/1     1            1           54d
deployment.apps/argocd-notifications-controller    1/1     1            1           54d
deployment.apps/argocd-redis                       1/1     1            1           54d
deployment.apps/argocd-repo-server                 1/1     1            1           54d
deployment.apps/argocd-server                      1/1     1            1           54d

NAME                                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-587588cd69   1         1         1       33d
replicaset.apps/argocd-dex-server-5fbb6d7489                  1         1         1       33d
replicaset.apps/argocd-notifications-controller-7bf9b85f77    1         1         1       33d
replicaset.apps/argocd-redis-598f75bc69                       1         1         1       54d
replicaset.apps/argocd-repo-server-75786b8cb                  1         1         1       33d
replicaset.apps/argocd-server-5dc57779d8                      1         1         1       33d

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     54d
```

## Step 4: Verify the installation

Once the installation is complete, verify that Argo CD is up and running by checking the status of its pods:

```console
$ kubectl get pods -n argocd
```
This command will list all the pods running in the argocd namespace. Make sure that all the pods are in a "Running" state.

## Step 5: Access the Argo CD UI

Get argocd service details:

```console
$ kubectl get svc -n argocd
NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.111.152.16   <none>        7000/TCP,8080/TCP            54d
argocd-dex-server                         ClusterIP   10.97.164.101   <none>        5556/TCP,5557/TCP,5558/TCP   54d
argocd-metrics                            ClusterIP   10.108.235.53   <none>        8082/TCP                     54d
argocd-notifications-controller-metrics   ClusterIP   10.108.206.9    <none>        9001/TCP                     54d
argocd-redis                              ClusterIP   10.97.10.198    <none>        6379/TCP                     54d
argocd-repo-server                        ClusterIP   10.107.234.9    <none>        8081/TCP,8084/TCP            54d
argocd-server                             ClusterIP   10.99.137.9     <none>        80/TCP,443/TCP               54d
argocd-server-metrics                     ClusterIP   10.103.15.175   <none>        8083/TCP                     54d
```

Finally, access the Argo CD UI using a web browser. To access the UI, we need to create a port-forwarding connection to the Argo CD server. Use the following command to create a port-forwarding connection:
```console
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
```

This command will create a local port forwarding connection from machine to the Argo CD server. Once the connection is established, open a web browser and navigate to `https://localhost:8080`.

![Alt text](https://www.bogotobogo.com/DevOps/Docker/images/Docker_Kubernetes_ArcoCD/ArcoCD-8080.png)

## Step 6: Log in using the default username and password:
- `Username`: admin
- `Password`: The password is autogenerated and can be obtained by running the following command:

Way 1:
```console
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

OR

Way 2:
```console
$ kubectl get secrets -n argocd
NAME                              TYPE     DATA   AGE
argocd-initial-admin-secret       Opaque   1      54d
argocd-notifications-secret       Opaque   0      55d
argocd-secret                     Opaque   5      55d
cluster-192.168.64.3-1062665955   Opaque   3      54d
repo-1269602644                   Opaque   4      52d
repo-2914916289                   Opaque   4      52d


$ kubectl get secrets argocd-initial-admin-secret -n argocd -o yaml
apiVersion: v1
data:
  password: <base64 encoded password>
kind: Secret
metadata:
  creationTimestamp: "2023-01-31T06:20:05Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "1244"
  uid: 81914138-d5eb-4d2d-bc14-212d4b500143
type: Opaque

$echo <base64 encoded password> | base64 -d
<argocd_password>                     

```

## `We have Successfully Logged in into ArgoCD WebUI !`


## Installing Argo CD CLI

The Argo CD CLI is a command-line tool that allows us to interact with the Argo CD server. Here's how to install it on system:

1. `Go to argocd github release --> Assests --> Copy link address(argocd-linux-amd64)`
   Open a terminal and use following command to download package.
```console
$ wget <copied-url>
```

2. Rename package as `argocd`
```console
$ mv argocd-linux-amd64 argocd
```

3. move package in `/usr/local/bin/argocd`
```console
$ mv argocd /usr/local/bin/
```

4. Make the argocd binary executable by running the following command:
```console
$ chmod +x /usr/local/bin/argocd
```

To verify that the installation was successful, run the following command:
```console
$ argocd version

argocd: v2.3.13+eb7b8a4
  BuildDate: 2023-01-18T03:48:53Z
  GitCommit: eb7b8a47908e01f032a29a16eae782dac6702638
  GitTreeState: clean
  GoVersion: go1.17.13
  Compiler: gc
  Platform: darwin/amd64
argocd-server: v2.6.2+6e02f8b

```
this command should print the version of the Argo CD CLI that just installed.

That's it! We have successfully installed the Argo CD CLI on system. We can now use the `argocd` command to interact with Argo CD server.
