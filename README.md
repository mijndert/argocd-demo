# ArgoCD Demo

## Install Kubernetes locally

Install [Docker Desktop](https://www.docker.com/products/docker-desktop) first and enable Kubernetes in the preferences. This will give you a local Kubernetes cluster to work with.

## Install ArgoCD on your Kubernetes cluster

First, clone this Git repository.

Create a namespace for ArgoCD to live in.

```
kubectl create namespace argocd
```

And install ArgoCD into the namespace.

```
kubectl -n argocd apply -f ./argocd/install.yaml
```

## Port forward the dashboard

Because this is a demo I'm not gonna bother with a proper load balancer or ingress controller - I'm just going to temporarily port forward the dashboard.

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

You can now access the dashboard via [localhost](http://localhost:8080/) (ignore any SSL warnings).

The default username is `admin` and the password is the name of the ArgoCD API pod, for example: `argocd-server-6744f57d55-26ctk`. 

You can look up the pod name using:

```
kubectl get pods -n argocd
```

## Deploy Nginx

Now we can deploy the example deployment I created, it will deploy Nginx.

```
kubectl apply -n argocd -f ./argocd/nginx.yaml
```

Any changes that you push to the Git repository will be automatically deployed because ArgoCD constantly watches for changes. ArgoCD will also self-heal all parts of the deployment if it detects anything is out of sync.

## Useful links

- [ArgoCD documentation](https://argoproj.github.io/argo-cd/)
