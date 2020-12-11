# Config File Reference

Starting from K3ai 0.2.0 we introduced a configuration file to allow a user to deploy various flavors of Kubernetes both locally and remotely.

The config file is typically saved in `<yourhomedir>/.k3ai` but could be moved around and/or hosted on other locations. We currently do not support remote config files. 

A single config file may host multiple configurations, hence is capable to deploy multiple clusters at the same time.

## Generic Section

```yaml
kind: cluster
targetCustomizations:
```

The kind definition at the beginning of the config file indicates that all the pieces of information below are relative to the infrastructure deployment. We did this with the intention later to have the capability to split the config behavior and possibly call other config files. 

## Common Sections

```yaml
kind: cluster
targetCustomizations:
- name: localK3s #name of the cluster instance not the name of the cluster
  enabled: false
  type: k3s
  ...
  clusterName: demo-wsl-k3s 
  clusterDeployment: local
  
  spec:
    wsl: "https://github.com/rancher/k3s/releases/download/v1.19.4%2Bk3s1/k3s"
    mac: 
    linux: "https://get.k3s.io | K3S_KUBECONFIG_MODE=644 sh -s -"
    windows: 
  plugins: 
  - repo: 
    name: 
  - repo: 
    name: 
```

* **name**: is the instance name as an internal reference for K3ai. Is not currently used so it act as a placeholder right now
* **enabled**: if set to true the section will be used and the cluster will be deployed
* **type**: represent the cluster to be installed: k3s,k3d,k0s,kinD
* **clusterName**: this is the name of the cluster \(if applicable\), it's useful to deploy multiple clusters of the same type
* **clusterDeployment**: local or cloud. For the cloud specs see below.
* **spec**: For each deployment, there are various options and binaries so dependening of where you're going to install we will take care of the right version+location. This is also useful if you want to test on a specific version.
* **plugins**: repo is the URL where the plugin are hosted. If you are using the public ones you may leave it empty, name is the name of the plugin as it appears from `k3ai-cli list` currently we do not support groups yet in the config file.

### Rancher K3s Config Specifics

```yaml
...
  type: k3s
    #default location of config file or your existing config file to copy
    config: "/etc/rancher/k3s/k3s.yaml" 
...
  clusterStart: "sudo bash -ic 'k3s server --write-kubeconfig-mode 644 > /dev/null 2>&1 &'"

```

### Rancher K3d Config Specifics

```yaml
 type: k3d
...
  clusterStart: "k3d cluster create"
  spec:
    wsl: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    mac: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-darwin-amd64"
    linux: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    windows: "https://github.com/rancher/k3d/releases/download/v3.4.0-test.0/k3d-windows-amd64.exe"
```

### Mirantis K0s Config Specifics

```yaml
type: k0s
    #default location of config file or your existing config file to copy
    config: "${HOME}/.k3ai/kubeconfig" 
...
  clusterStart: "k0s default-config | tee ${HOME}/.k3ai/k0s.yaml && 
  sudo bash -ic 'k0s server -c ${HOME}/.k3ai/k0s.yaml --enable-worker > /dev/null 2>&1 &' &&
   sudo cat /var/lib/k0s/pki/admin.conf > $HOME/.k3ai/k0s-config"
  spec:
```

Do not copy the above, has been truncated to make it more readable.

### KinD Config Specifics

```yaml
type: kind
  config:  
...
  clusterStart: "kind create cluster"
  spec:
    wsl: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    mac: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-darwin-amd64"
    linux: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    windows: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-windows-amd64"
```

### Remote Clusters Specifics

```yaml
enabled: false
  clusterDeployment: cloud
  clusterStart: 
  spec:
    wsl: 
    mac: 
    linux:
    windows:
    cloudType: civo
    cloudNodes: 1
    cloudSecretPath: $HOME/.k3ai/secret.txt
```

We currently support only Civo Cloud. Notice that the cloudSecretPath is a placeholder we are going to add this feature but for the time being, you'll need to pass the key through the terminal directly.

