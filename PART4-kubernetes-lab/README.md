# Kubernetes lab

## Setup Kubernetes

Prerequisites:
1. Download the CLI tool: `kubectl`
2. Kubernetes cluster configuration and access

### Download the CLI tool: `kubectl`

Open powershell and run:

```powershell
(New-Object System.Net.WebClient).DownloadFile('https://storage.googleapis.com/kubernetes-release/release/v1.10.0/bin/windows/amd64/kubectl.exe', (Join-Path $PWD 'kubectl.exe'))
```

### Kubernetes cluster configuration and access

Download the Kubernetes `config` from `#dockerton` (Slack) and move the file to `~/.kube/config`.

### Test the CLI tool and configuration

View nodes:
```bash
kubectl.exe get nodes
```

## Create __team__ namespace in Kubernetes

### View namespaces:
```bash
kubectl.exe get namespaces
```

### Edit `k8s/namespace.yml`:
* Replace `##namespace##` with team name.

### Create namespace:
```bash
kubectl.exe apply -f ./k8s/namespace.yml
```

### Verify new namespace exists:
```bash
kubectl.exe get namespaces
```

### Change from _default_ namespace to _team_ namespace:
```powershell
.\kubectl.exe config set-context $(.\kubectl.exe config current-context) --namespace <team_name>
```

## Create __team__ deployment in Kubernetes

### View deployments:
```bash
kubectl.exe get deployments
```

### Edit `k8s/deplyoment.yml`.
* Replace `##namespace##` with team name.
* Replace `##application-name##` with the name of your application
* Replace `##image##` with the Docker image (`repository/name:tag`)

### Create deployment:
```bash
kubectl.exe apply -f ./k8s/deployment.yml
```

### Verify deployment:
```bash
kubectl.exe get deployments
kubectl.exe describe deployments
```

### Verify pods:
```bash
kubectl.exe get pods
kubectl.exe describe pods
```

### "Debug" pod:
```bash
kubectl.exe port-forward <pod-name> 8080:80
```

Open http://localhost:8080/api/values in browsers.

## Create __team__ service in Kubernetes

### View services:
```bash
kubectl.exe get services
```

### Edit `k8s/service.yml`.
* Replace `##namespace##` with team name.
* Replace `##application-name##` with the name of your application

### Create service:
```bash
kubectl.exe apply -f ./k8s/service.yml
```

### Verify service:
```bash
kubectl.exe get service
kubectl.exe describe service
```

### "Debug" service:
```bash
kubectl.exe proxy
```
Open http://localhost:8001/api/v1/namespaces/{team_name}/services/{application_name}:80/proxy/api/values in browsers.

## Create __team__ ingress in Kubernetes

### View ingress:
```bash
kubectl.exe get ingress
```

### Edit `k8s/ingress.yml`.
* Replace `##namespace##` with team name.
* Replace `##application-name##` with the name of your application
* **NOTE**: remember the `/` prefix on the path entry

### Create ingress:
```bash
kubectl.exe apply -f ./k8s/ingress.yml
```

### Verify ingress:
```bash
kubectl.exe get ingress
kubectl.exe describe ingress
```

### "Debug" ingress:
Open http://dockerton.onep.dk/{application_name}/api/values in browsers.

## Misc

* [kubectl.exe cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
