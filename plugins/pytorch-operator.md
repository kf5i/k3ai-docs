---
description: Kubeflow PyTorch-Job Training Operator
---

# PyTorch Operator

PyTorch is a Python package that provides two high-level features:

* Tensor computation \(like NumPy\) with strong GPU acceleration
* Deep neural networks built on a tape-based autograd system

You can reuse your favorite Python packages such as NumPy, SciPy, and Cython to extend PyTorch when needed. More information at [**https://github.com/kubeflow/pytorch-operator**](https://github.com/kubeflow/pytorch-operator) _\*\*_or the PyTorch site [https://pytorch.org/](https://pytorch.org/)

## Quick Start

As usual, let's deploy PyTorch with one single line command

If you leverage **CPU** only

```text
curl -sfL https://get.k3ai.in | bash -s -- --cpu  --plugin_pytorch-operator
```

if you like to use PyTorch with **GPU**

```text
curl -sfL https://get.k3ai.in | bash -s -- --gpu --plugin_pytorch-operator
```

## Test You PyTorch-Job installation

We will use the MNISE example from the Kubeflow PyTorch-Job repo at [**https://github.com/kubeflow/pytorch-operator/tree/master/examples/mnist**](https://github.com/kubeflow/pytorch-operator/tree/master/examples/mnist)\*\*\*\*

As usual, we want to avoid complexity so we re-worked a bit the sample and make it way much more easier.

### Step 1

You'll see tha in the example a container need to be created before running the sample, we merged the container commands directly in the **YAML** file so now it's one-click job.

For **CPU only**

```yaml
k3s kubectl apply -f - << EOF
apiVersion: "kubeflow.org/v1"
kind: "PyTorchJob"
metadata:
  name: "pytorch-dist-mnist-gloo"
  namespace: kubeflow
spec:
  pytorchReplicaSpecs:
    Master:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: pytorch
              image: pytorch/pytorch:1.0-cuda10.0-cudnn7-runtime
              command: ['sh','-c','pip install tensorboardX==1.6.0 && mkdir -p /opt/mnist/src && cd /opt/mnist/src && curl -O https://raw.githubusercontent.com/kubeflow/pytorch-operator/master/examples/mnist/mnist.py && chgrp -R 0 /opt/mnist && chmod -R g+rwX /opt/mnist && python /opt/mnist/src/mnist.py']
              args: ["--backend", "gloo"]

    Worker:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: pytorch
              image: pytorch/pytorch:1.0-cuda10.0-cudnn7-runtime
              command: ['sh','-c','pip install tensorboardX==1.6.0 && mkdir -p /opt/mnist/src && cd /opt/mnist/src && curl -O https://raw.githubusercontent.com/kubeflow/pytorch-operator/master/examples/mnist/mnist.py && chgrp -R 0 /opt/mnist && chmod -R g+rwX /opt/mnist && python /opt/mnist/src/mnist.py']
              args: ["--backend", "gloo"]
EOF
```

If you have **GPU** enabled you may run it this way

```yaml
k3s kubectl apply -f - << EOF
apiVersion: "kubeflow.org/v1"
kind: "PyTorchJob"
metadata:
  name: "pytorch-dist-mnist-gloo"
  namespace: kubeflow
spec:
  pytorchReplicaSpecs:
    Master:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: pytorch
              image: pytorch/pytorch:1.0-cuda10.0-cudnn7-runtime
              command: ['sh','-c','pip install tensorboardX==1.6.0 && mkdir -p /opt/mnist/src && cd /opt/mnist/src && curl -O https://raw.githubusercontent.com/kubeflow/pytorch-operator/master/examples/mnist/mnist.py && chgrp -R 0 /opt/mnist && chmod -R g+rwX /opt/mnist && python /opt/mnist/src/mnist.py']
              args: ["--backend", "gloo"]
              # Change the value of nvidia.com/gpu based on your configuration
              resources:
                limits:
                  nvidia.com/gpu: 1 
    Worker:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: pytorch
              image: pytorch/pytorch:1.0-cuda10.0-cudnn7-runtime
              command: ['sh','-c','pip install tensorboardX==1.6.0 && mkdir -p /opt/mnist/src && cd /opt/mnist/src && curl -O https://raw.githubusercontent.com/kubeflow/pytorch-operator/master/examples/mnist/mnist.py && chgrp -R 0 /opt/mnist && chmod -R g+rwX /opt/mnist && python /opt/mnist/src/mnist.py']
              args: ["--backend", "gloo"]
              # Change the value of nvidia.com/gpu based on your configuration
              resources:
                limits:
                  nvidia.com/gpu: 1 
EOF
```

### Step 2

Check if pod are deployed correctly with

```bash
kubectl get pod -l pytorch-job-name=pytorch-dist-mnist-gloo -n kubeflow
```

It should ouput something like this

```bash
NAME                               READY   STATUS    RESTARTS   AGE
pytorch-dist-mnist-gloo-master-0   1/1     Running   0          2m26s
pytorch-dist-mnist-gloo-worker-0   1/1     Running   0          2m26s
```

### Step 3

Check logs result of your training job

```bash
 kubectl logs -l pytorch-job-name=pytorch-dist-mnist-gloo -n kubeflow
```

You should observe an output similar to this \(since we are using 1 Master and 1 worker in this case\)

```bash
Train Epoch: 1 [55680/60000 (93%)]      loss=0.0341
Train Epoch: 1 [56320/60000 (94%)]      loss=0.0357
Train Epoch: 1 [56960/60000 (95%)]      loss=0.0774
Train Epoch: 1 [57600/60000 (96%)]      loss=0.1186
Train Epoch: 1 [58240/60000 (97%)]      loss=0.1927
Train Epoch: 1 [58880/60000 (98%)]      loss=0.2050
Train Epoch: 1 [59520/60000 (99%)]      loss=0.0642

accuracy=0.9660

Train Epoch: 1 [55680/60000 (93%)]      loss=0.0341
Train Epoch: 1 [56320/60000 (94%)]      loss=0.0357
Train Epoch: 1 [56960/60000 (95%)]      loss=0.0774
Train Epoch: 1 [57600/60000 (96%)]      loss=0.1186
Train Epoch: 1 [58240/60000 (97%)]      loss=0.1927
Train Epoch: 1 [58880/60000 (98%)]      loss=0.2050
Train Epoch: 1 [59520/60000 (99%)]      loss=0.0642

accuracy=0.9660
```

