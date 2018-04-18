# *Dockerton* 2018

## Tools/Prerequisites

### Install .NET Core SDK

Download .NET Core SDK from [here](https://www.microsoft.com/net/download/thank-you/dotnet-sdk-2.1.104-windows-x64-installer)  and run the installer.

### Download `kubectl`

Open powershell and run:

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.10.0/bin/windows/amd64/kubectl.exe
```

[kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

### Copy `config` to `~/.kube/config`

Copy...

---

## Work items

* [ALL] Prepare README (THIS!!)
* [RU] Setup demo cluster
  * [ALL] Distribute Kubernetes `config`
* Helm
  * Install Tiller on demo cluster
* [JA] Logging
  * [JA] FluentD
  * [JA] Elastic (Helm)
  * [JA] Kibana
* [TH] Monitoring
  * [TH] Helm chart
    * [TH] Investigate
  * [TH] Hand-rolled
    * [TH] Prometheus
    * [TH] NodeExporter
    * [TH] Grafana
    * [TH] Kube State Metrics
* [JA] Docker registry
  * Setup in demo cluster with Helm chart
* [✓] Overall ingress
  * [✓] ELB + certificate
  * [✓] Setup Traefik
* [✓] Prepare Kubernetes Manifests
  * [✓] Namespace
  * [✓] Deployment
  * [✓] Service
  * [✓] Ingress
