

# Installing latest/stable version of ArgoCD
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Forward Ports
```
k get services -n argocd
kubectl port-forward service/argocd-server -n argocd 8080:443
```

### Get Credentials
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

# Install ArgoCD CLI / Login via CLI
```
brew install argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443
argocd login 127.0.0.1:8080
```

# Creating an Application using ArgoCD CLI:
```
argocd app create webapp-kustom-prod \
--repo https://github.com/devopsjourney1/argo-examples.git \
--path kustom-webapp/overlays/prod --dest-server https://kubernetes.default.svc \
--dest-namespace prod
```

# Command Cheat sheet
```
argocd app create #Create a new Argo CD application.
argocd app list #List all applications in Argo CD.
argocd app logs <appname> #Get the application’s log output.
argocd app get <appname> #Get information about an Argo CD application.
argocd app diff <appname> #Compare the application’s configuration to its source repository.
argocd app sync <appname> #Synchronize the application with its source repository.
argocd app history <appname> #Get information about an Argo CD application.
argocd app rollback <appname> #Rollback to a previous version
argocd app set <appname> #Set the application’s configuration.
argocd app delete <appname> #Delete an Argo CD application.

Best practices is to have two repositories, 
one containg our application source code and 
another containg the configuration code which contains our k8s assets
such as deployments, services and configuration Maps
Whenever the configuration repo gets updated then Argocd goes in 
and deploy to Kubernetes cluster
This is at the CD end, we still require the CI pipeline to do the CI part berfore the CD can be initiated
This will build and test our application, build image and push to the image repo like Dockerhub
The CI pipeline then triggers update to the configuration repo and then ArgoCd kicks in and do the sync
Configuration Repo is the Source of truth as it contains the Desired State
The kubernetes Cluster is the actual state, the Argocd makes sure that the two states must always be in sync
ArgoCd can be installed on one cluster but controls the configuration of multile cluster that is it is not restricted to the cluster in which it is installed
If the cluster in which Argocd is installed fails, it can be recovered because the configuration of Argocd is installed as a Yaml file

```
Demo video: https://www.youtube.com/watch?v=JLrR9RV9AFA




