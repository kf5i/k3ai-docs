# How to Build Your First Plugin

Building plugins for k3ai is very simple. This guide will walk you through the steps required to build a plugin and contribute to the project.

The first step is to learn the structure of the plugins repository: [**https://github.com/kf5i/k3ai-plugins**](https://github.com/kf5i/k3ai-plugins)\*\*\*\*

The repo is structured in a very simple way. 

* core
  * groups
  * plugins
* common
* community

**`Core`**is the root folder it includes  **plugins**  and  **groups**

**`Plugins`** are the actual application to be deployed, for each plugin folder there is a plugin.yaml file.

**Groups** are a combination of various plugins to be installed altogether

Under **`common`** you'll find all the manifests or files needed by more than one plugin. Those are sort of reusable components \(i.e.: treafik ingress definitions for plugins\).

k3ai supports custom repositories for plugins and groups so this means you may have your own instead of using our public ones.

Let's create a local repo and deploy a "hello-world" plugin.

First, we have to create the basic structure.  So let's create anywhere on your laptop a structure like this:

* demo
  * core
    * groups
    * plugins
      * demo-plugin
        * plugin.yaml
  * common
    * demo-plugin
      * deployment.yaml

Now let's open the plugin.yaml file and copy the below content in it.

```yaml
plugin-name: demo-plugin
plugin-description: Demo of a custom local plugin
namespace: "default"
yaml:
  - url: "./commons/demo-plugin/deployment.yaml"
    type: "file"
```

Save the file and open the deployment.yaml. Copy&Paste the following content.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: shell-demo
spec:
  volumes:
  - name: shared-data
    emptyDir: {}
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html
  hostNetwork: true
  dnsPolicy: Default
```

We are ready let's check the plugin list with

```yaml
k3ai-cli list --repo "<absolute path to root folder>/"

#k3ai-cli list --repo "home/user/core/"
```

You should get something like this

```yaml
k3ai-cli list --repo "<absolute path to root folder>/"

Name                           Description
demo-plugin                    A simple demo of a local plugin
```

Great! Now let's apply the plugin to our environment

```yaml
k3ai-cli apply --repo "<absolute path to root folder>/"
```

Now let's check if the pod is running with

```yaml
kubectl get pod shell-demo

#Output
NAME         READY   STATUS    RESTARTS   AGE
shell-demo   1/1     Running   0          3m9s
```

We are ready so let's execute a command inside our plugin. Copy and Paste the below command into your terminal.

```yaml
kubectl exec --stdin --tty shell-demo -- /bin/bash -c "apt-get update > /dev/null && apt-get -y install boxes > /dev/null &&  echo 'Hey, this is K3ai! Thanks for use this.' | boxes -d peek"
```

If everything goes right you should see something like this

```yaml
/*       _\|/_
         (o o)
 +----oOO-{_}-OOo------------------------+
 |Hey, this is K3ai! Thanks for use this.|
 +--------------------------------------*/
```

Congratulation you created your first plugin. Now to delete it simply execute

```yaml
k3ai-cli delete --repo "<absolute path to root folder>/"
```

