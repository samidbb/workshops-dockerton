# Kubernetes batteries included

## Setup Kubernetes

> NOTE: Copy `kubectl.exe` from previous exercise or download again. Skip the configuration step (2).

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

## Logging

## Monitoring

## Scaling

## Failure recovery
