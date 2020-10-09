---
description: >-
  Civo was born when our small team first came together to create an
  OpenStack-based cloud for a shared hosting provider. Read their story here:
  https://www.civo.com/blog/kube100-so-far
---

# Civo Cloud

[In 2019 we went all in and took Civo in a new direction](https://www.civo.com/blog/a-civo-2019-retrospective-how-we-got-here-and-what-s-next), launching the world’s first k3s-powered, managed Kubernetes service into beta.

As a natural as it could be, we work perfectly on Civo so here it is the simples guide ever to run our k3ai on it, three steps and your k3ai is ready!

### Installing k3ai on Civo

Ready? It will require no more than 5 minutes to do it!

You have to have an account on Civo.com, to do so simply register on Civo here: 

[**https://www.civo.com/signup**](https://www.civo.com/signup)\*\*\*\*

### Step 1

Launch your k3s cluster using the default options \(Traefik and Metric-server selected\)

![k3s selection](../.gitbook/assets/1.png)

Wait for the instance to finish the deployment

![k3s deployment](../.gitbook/assets/4.png)

### Step 2

Download the **kubeconfig** file,  move it to your preferred location, and set your environment to use it:

```bash
kubectl config --kubeconfig="civo-k3ai-kubeconfig"
```

### Step 3

You're all set up one more thing and we have done:

```bash
 curl -sfL https://github.com/kf5i/k3ai/releases/latest/download/install | bash -s - --skipk3s --plugin_civo_kfpipelines
```

enjoy your k3ai on[ **https://civo.com**](https://civo.com)\*\*\*\*

