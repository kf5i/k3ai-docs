---
description: Kubeflow Tensorflow-Job Training Operator
---

# Tensorflow Operator

TFJob provides a Kubernetes custom resource that makes it easy to run distributed or non-distributed TensorFlow jobs on Kubernetes.

More on the Tensorflow Operator at [**https://github.com/kubeflow/tf-operator**](https://github.com/kubeflow/tf-operator)\*\*\*\*

## Quick Start

All you have to run is with **CPU** support

```bash
curl -sfL https://get.k3ai.in | bash -s -- --cpu --plugin_tf-operator
```

to run with **GPU** support

```bash
curl -sfL https://get.k3ai.in | bash -s -- --gpu--plugin_tf-operator
```

## Test your installation

We present here a sample from Tensorflow Operator on [**https://github.com/kubeflow/tf-operator**](https://github.com/kubeflow/tf-operator)\*\*\*\*

### Step 1

We first need to add a persistent volume and claim, to do so let's add the two **YAML** file we need, copy and paste each command in order.

```yaml
k3s kubectl apply -f - << EOF
apiVersion: v1
kind: PersistentVolume
metadata:
  name: tfevent-volume
  labels:
    type: local
    app: tfjob
spec:
  capacity:
    storage: 10Gi
  storageClassName: local-path
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /tmp/data
EOF
```

now we add the **PVC**.

```yaml
k3s kubectl apply -f - << EOF
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: tfevent-volume
  namespace: kubeflow 
  labels:
    type: local
    app: tfjob
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi
EOF
```

_Note: Because we are using local-path as storage volume and we are on a single node cluster we can't use ReadWriteMany as per Rancher local-path provisioner issue_ [_https://github.com/rancher/local-path-provisioner/issues/70\#issuecomment-574390050_](https://github.com/rancher/local-path-provisioner/issues/70#issuecomment-574390050)\_\_

## Step 2

Now we deploy the example

```yaml
kubectl apply -f https://raw.githubusercontent.com/kubeflow/tf-operator/master/examples/v1/mnist_with_summaries/tf_job_mnist.yaml
```

You can observe the result of the example with

```yaml
kubectl logs -l tf-job-name=mnist -n kubeflow --tail=-1
```

It should output something similar to this \(we show just partially the output here\)

```yaml
...
Adding run metadata for 799
Accuracy at step 800: 0.957
Accuracy at step 810: 0.9698
Accuracy at step 820: 0.9676
Accuracy at step 830: 0.9676
Accuracy at step 840: 0.9677
Accuracy at step 850: 0.9673
Accuracy at step 860: 0.9676
Accuracy at step 870: 0.9654
Accuracy at step 880: 0.9694
Accuracy at step 890: 0.9708
Adding run metadata for 899
Accuracy at step 900: 0.9737
Accuracy at step 910: 0.9708
Accuracy at step 920: 0.9721
Accuracy at step 930: 0.972
Accuracy at step 940: 0.9639
Accuracy at step 950: 0.966
Accuracy at step 960: 0.9654
Accuracy at step 970: 0.9683
Accuracy at step 980: 0.9685
Accuracy at step 990: 0.9666
Adding run metadata for 999
```

