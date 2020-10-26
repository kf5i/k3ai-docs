# How to Build Your First Plugin

Building plugins for k3ai is very simple. This guide will walk you through the steps required to build oa plugin and contribute to the project.

The first step is to learn the structure of the plugins repository: [**https://github.com/kf5i/k3ai-plugins**](https://github.com/kf5i/k3ai-plugins)\*\*\*\*

The repo is structured in a very simple way. For each plugin you have a corresponding folder named:

**plugin\_&lt;PLUGIN NAME&gt;**

where **&lt;PLUGIN NAME&gt;** is the name of your plugin \(i.e.: plugin\_kfpipelines identify the Kubeflow pipelines based on default Argo workflows engine\).

The plugin folder should contain two files:

* README.md
* install

_Notice the absence of an extension for the install file._

The **plugin\_template** contains a starting point for contributors.

In a nutshell, the **install** file is a bash file where you can store your logic. See the reference template install file below.

The **install** is organized in three parts. The initial part is useful to check container status and pass pieces of information in the terminal.

```bash
#!/bin/bash

#########################################
### K3ai (keɪ3ai) Plugins - Tensorflow Serving
### https://github.com/kf5i/k3ai
### Alessandro Festa @bringyourownai
### Gabriele Santomaggio @gsantomaggio
######################################### 

info()
{
    echo '[INFO] ' "$@"
}

infoL()
{
    echo -en '[INFO] ' "$@\n"
}

sleep_cursor()
{
 chars="/-\|"
 for (( z=0; z<7; z++ )); do
   for (( i=0; i<${#chars}; i++ )); do
    sleep 0.5
    echo -en "${chars:$i:1}" "\r"
  done
done
}


wait() 
{
status=1
infoL "Testing.." $1.$2  
while [ : ]
  do
    sleep_cursor &
    kubectl wait --for condition=ready --timeout=14s pod -l  $1   -n $2
    status=$?

    if [ $status -ne 0 ]
    then 
      infoL "$1 isn't ready yet. This may take a few minutes..."
      sleep_cursor
    else
      break  
    fi 
  done
}
```

The core of the plugin is in the **install\(\)** function

```bash
#######
### Kubeflow pipelines login as example, change it accordingly with your needs
install(){
    info "Installing <TEMPLATE NAME> crd"
    export PIPELINE_VERSION=1.0.1
    kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION"
    kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
    sleep_cursor &
    info "Installing pipelines manifests"
    kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic-pns?ref=$PIPELINE_VERSION"
##We create an Array to check when pods are available to system
    waiting_pod_array=("k8s-app=kube-dns;kube-system" 
                       "k8s-app=metrics-server;kube-system"
                       "app=traefik;kube-system"  
                       "app=minio;kubeflow"
                       "app=mysql;kubeflow"
                       "app=cache-server;kubeflow"
                       "app=ml-pipeline-persistenceagent;kubeflow"
                       "component=metadata-grpc-server;kubeflow"
                       "app=ml-pipeline-ui;kubeflow")

    for i in "${waiting_pod_array[@]}"; do 
      echo "$i"; 
      IFS=';' read -ra VALUES <<< "$i"
        wait "${VALUES[0]}" "${VALUES[1]}"
    done
#####



    info "Kubeflow pipelines ready!!"
#### Ingress on Treafik definition if not needed (i.e. your plugin install Istio)
# remove it, your PR will probably need more work in that case beacuse k3s by 
# default install Traefik.

    info "Defining the ingress"
    sleep_cursor

    kubectl apply -f - << EOF
      apiVersion: networking.k8s.io/v1beta1
      kind: IngressClass
      metadata: 
        name: traefik-lb
      spec: 
        controller: traefik.io/ingress-controller
EOF

    kubectl apply -f - << EOF
      apiVersion: "networking.k8s.io/v1beta1"
      kind: "Ingress"
      metadata:
        name: "pipeline-ingress"
        namespace: kubeflow
        annotations:
          nginx.ingress.kubernetes.io/rewrite-target: /$2

      spec:
        ingressClassName: "traefik-lb"
        rules:
        - http:
            paths:
            - path: "/"
              backend:
                serviceName: "ml-pipeline-ui"
                servicePort: 80
EOF
```

the last part is where you call back the **install** function

```bash
sleep_cursor

IP=$(kubectl get service/traefik -o jsonpath='{.status.loadBalancer.ingress[0].ip}' -n kube-system)
info "pipelines UI: http://"$IP 
}

install
```

