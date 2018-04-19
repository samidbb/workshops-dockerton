# Kubernetes lab

## Setup Kubernetes

Prerequisites:
1. Download the CLI tool: `kubectl`
2. Kubernetes cluster configuration and access

### Download the CLI tool: `kubectl`

Open powershell (in the project root) and run:

```powershell
choco install kubernetes-cli
```

If [Chocolatey](https://chocolatey.org/) is installed on the system (`kubectl` will be available globally), otherwise:

```powershell
(New-Object System.Net.WebClient).DownloadFile('https://storage.googleapis.com/kubernetes-release/release/v1.10.0/bin/windows/amd64/kubectl.exe', (Join-Path $PWD 'kubectl.exe'))
```

and run `kubectl` from the current location.

### Kubernetes cluster configuration and access

Download the Kubernetes `config` from `#dockerton` (Slack) and move the file to the `~/.kube/` folder.

```powershell
cd ~; mkdir .kube; cp ~/Downloads/config.yaml ~/.kube/config
```

> ##### A note about the path `~/.kube/`
> The `~/` is the current user's home directory (`$HOME` in powershell and `%userprofile%` in DOS), which is also understood by powershell. 
> It may not possible to create a `.kube` folder using explorer - powershell to the rescue...

### Test the CLI tool and configuration

#### Get the client and server version:
```bash
kubectl.exe version
```

Expected output:
```bash
Client Version: version.Info{Major:"1", Minor:"9", GitVersion:"v1.9.3", GitCommit:"d2835416544f298c919e2ead3be3d0864b52323b", GitTreeState:"clean", BuildDate:"2018-02-09T21:51:54Z", GoVersion:"go1.9.4", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"8", GitVersion:"v1.8.7", GitCommit:"b30876a5539f09684ff9fde266fda10b37738c9c", GitTreeState:"clean", BuildDate:"2018-01-16T21:52:38Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"linux/amd64"}
```

> If the server version is missing there is most likely an issue with the configuration above.

#### View nodes:
```bash
kubectl.exe get nodes
```

Expected output:
```bash
NAME                                             STATUS    ROLES     AGE       VERSION
ip-172-20-40-213.eu-central-1.compute.internal   Ready     node      1d        v1.8.7
ip-172-20-44-227.eu-central-1.compute.internal   Ready     node      1d        v1.8.7
ip-172-20-45-131.eu-central-1.compute.internal   Ready     master    1d        v1.8.7
ip-172-20-59-136.eu-central-1.compute.internal   Ready     node      1d        v1.8.7
```

## Create __team__ namespace in Kubernetes

### View namespaces:
```bash
kubectl.exe get namespaces
```

Expected output:
```bash
NAME                     STATUS    AGE
default                  Active    1d
dockerregistry           Active    1d
elasticsearch            Active    20h
kube-public              Active    1d
kube-system              Active    1d
monitoring               Active    21h
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
