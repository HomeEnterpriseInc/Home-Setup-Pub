# Home Setup

This is a project for starter to setup whole home in kubernetes infrastructure. Please contact me if you need my help to setup this project.

### Pre Requisites
* Hardware running Linux OS
* Kubernetes with worker nodes or standalone setup
* ArgoCD for gitops
* Metrics - I prefer datadog free tier
* cert-bot operator
* persistent volume

### Assumed Constants
* `ClusterIssuer` by name `letsencrypt`


### Steps to use this repository
1. Install Ubuntu Server & Open SSH & Docker
2. Install Kubernetes - K3s 
   ```shell
    curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --docker --disable traefik,local-storage,metrics-server
   ```
3. Install ArgoCD - 
   ```shell
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
4. Get ArgoCD secret 
    ```shell
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```
5. Expose ArgoCD UI - 
    ```shell
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
6. Access ArgoCD https://localhost:8080 with username `admin` and password got it in step 4
7. Configure Projects, Repositories
8. Fork the repository, make your changes and add it as repository
9. Create Application from Git and direct to `apps` folder and all the application should be created automatically. 
