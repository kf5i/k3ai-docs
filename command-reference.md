# Command Reference



This the list of all command available within k3ai-cli

To get the list simply type `k3ai-cli -h`

```text
 k3ai-cli is a lightweight infrastructure-in-a-box solution specifically built to
 install and configure AI tools and platforms in production environments on Edge
 and IoT devices as easily as local test environments.

Usage:
  k3ai-cli [command]

Available Commands:
  apply       Apply a plugin or a plugin group
  delete      Delete a plugin or a plugin group
  help        Help about any command
  init        Initialize K3ai Client
  list        List all plugins or plugin groups
  version     Print CLI version

Flags:
  -h, --help          help for k3ai-cli
      --repo string   URI for the plugins repository.  

Use "k3ai-cli [command] --help" for more information about a command.
```

### Apply

with apply you deploy the various plugins or group-plugins.

```text
Apply a plugin or a plugin group

Usage:
  k3ai-cli apply <plugin_name> [flags]

Flags:
  -g, --group   Apply a plugin group
  -h, --help    help for apply

Global Flags:
      --repo string   URI for the plugins repository.  
      (default "https://api.github.com/repos/kf5i/k3ai-plugins/contents/core/")
```

### Init

With init you may deploy a cluster either local or remote

```text
Initialize K3ai Client, allowing user to deploy a new K8's cluster, 
list plugins and groups

Usage:
  k3ai-cli init [flags]

Examples:
k3ai-cli init                                   #Will use config from $HOME/.k3ai/config.yaml and use interactive menus
k3ai-cli init --config /myfolder/myconfig.yaml  #Use a custom config.yaml in another location(local or remote)
k3ai-cli init --local k3s                       #Use config target marked local and of type k3s
k3ai-cli init --cloud civo                      #Use config target marked as cloud and of type civo

Flags:
      --cloud string    Options availabe for cloud providers
      --config string   Custom config file [default is $HOME/.k3ai/config.yaml] (default "/.k3ai/config.yaml")
  -h, --help            help for init
      --local string    Options availabe k3s,k0s,kind

Global Flags:
      --repo string   URI for the plugins repository.  
      (default "https://api.github.com/repos/kf5i/k3ai-plugins/contents/core/")
```

### List

List is used to retrieve the available plugins or group-plugins

```text
List all plugins or plugin groups

Usage:
  k3ai-cli list [flags]

Flags:
  -g, --group   List the plugin groups
  -h, --help    help for list

Global Flags:
      --repo string   URI for the plugins repository.  
      (default "https://api.github.com/repos/kf5i/k3ai-plugins/contents/core/")
```

### Delete

Delete is used to remove a deployed plugin

```text
Delete a plugin or a plugin group

Usage:
  k3ai-cli delete <plugin_name> [flags]

Flags:
  -g, --group   Delete a plugin group
  -h, --help    help for delete

Global Flags:
      --repo string   URI for the plugins repository.  
      (default "https://api.github.com/repos/kf5i/k3ai-plugins/contents/core/")
```

k3ai use a config file  to support auomated deployment of cluster through the init command.

The sample template install by defauly KinD

```yaml
kind: cluster
targetCustomizations:
- name: localK3s #name of the cluster instance not the name of the cluster
  enabled: false
  type: k3s
  config: "/etc/rancher/k3s/k3s.yaml" #default location of config file or your existing config file to copy
  clusterName: demo-wsl-k3s #name of the cluster (this need to be the same as in a config file)
  clusterDeployment: local
  clusterStart: "sudo bash -ic 'k3s server --write-kubeconfig-mode 644 > /dev/null 2>&1 &'"
  spec:
  # If the OS is not needed may be removed so the three below are mutually exclusive, if not needed set them to null or remove it
    wsl: "https://github.com/rancher/k3s/releases/download/v1.19.4%2Bk3s1/k3s"
    mac: 
    linux: "https://get.k3s.io | K3S_KUBECONFIG_MODE=644 sh -s -"
    windows: 
    # Everything from this repo will be ran in this cluster. You trust me right?
  plugins: 
  - repo: 
    name: 
  - repo: 
    name: 

- name: localK0s #name of the cluster instance not the name of the cluster
  enabled: false
  type: k0s
  config: "${HOME}/.k3ai/kubeconfig" #default location of config file or your existing config file to copy
  clusterName: demo-wsl-k0s #name of the cluster (this need to be the same as in a config file)
  clusterDeployment: local
  clusterStart: "k0s default-config | tee ${HOME}/.k3ai/k0s.yaml && sudo bash -ic 'k0s server -c ${HOME}/.k3ai/k0s.yaml --enable-worker > /dev/null 2>&1 &' && sudo cat /var/lib/k0s/pki/admin.conf > $HOME/.k3ai/k0s-config"
  spec:
  # If the OS is not needed may be removed so the three below are mutually exclusive, if not needed set them to null or remove it
    wsl: "https://github.com/k0sproject/k0s/releases/download/v0.8.1/k0s-v0.8.1-amd64"
    mac: 
    linux: "https://github.com/k0sproject/k0s/releases/download/v0.8.1/k0s-v0.8.1-amd64"
    windows:
    # Everything from this repo will be ran in this cluster. You trust me right?
  plugins: 
  - repo: 
    name: 
  - repo: 
    name: 

- name: localKind #name of the cluster instance not the name of the cluster
  enabled: true
  type: kind
  config:  #default location of config file or your existing config file to copy
  clusterName: demo-win-kind #name of the cluster (this need to be the same as in a config file)
  clusterDeployment: local
  clusterStart: "kind create cluster"
  spec:
  # If the OS is not needed may be removed so the three below are mutually exclusive, if not needed set them to null or remove it
    wsl: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    mac: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-darwin-amd64"
    linux: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    windows: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-windows-amd64"
    # Everything from this repo will be ran in this cluster. You trust me right?
  plugins: 
  - repo: 
    name: jupyter-minimal
  - repo: 
    name: 

- name: localK3d #name of the cluster instance not the name of the cluster
  enabled: false
  type: k3d
  config:  #default location of config file or your existing config file to copy
  clusterName: demo-win-k3d #name of the cluster (this need to be the same as in a config file)
  clusterDeployment: local
  clusterStart: "k3d cluster create"
  spec:
  # If the OS is not needed may be removed so the three below are mutually exclusive, if not needed set them to null or remove it
    wsl: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    mac: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-darwin-amd64"
    linux: "https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64"
    windows: "https://github.com/rancher/k3d/releases/download/v3.4.0-test.0/k3d-windows-amd64.exe"
    # Everything from this repo will be ran in this cluster. You trust me right?
  plugins: 
  - repo: 
    name: jupyter-minimal
  - repo: 
    name: 

- name: remoteK3s #name of the cluster instance not the name of the cluster
  enabled: false
  type: k3s
  config: remote #default location of config file or your existing config file to copy  if Remote will be copy from remote location
  clusterName: demo-cluster-remote #name of the cluster (this need to be the same as in a config file)
  clusterDeployment: cloud
  clusterStart: 
  spec:
  # If the OS is not needed may be removed so the three below are mutually exclusive, if not needed set them to null or remove it
    wsl: 
    mac: 
    linux:
    windows:
    cloudType: civo
    cloudNodes: 1
    cloudSecretPath: $HOME/.k3ai/secret.txt
    # Everything from this repo will be ran in this cluster. You trust me right?
  plugins: 
  - repo: "https://github.com/alfsuse/demo-plugins"
    name: "demo"
  - repo: "https://github.com/alfsuse/demo-plugins-2"
    name: "demo2"
```

