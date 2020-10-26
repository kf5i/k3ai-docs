# Tensorflow Serving - ResNet

TensorFlow Serving is a flexible, high-performance serving system for machine learning models, designed for production environments. TensorFlow Serving makes it easy to deploy new algorithms and experiments, while keeping the same server architecture and APIs. Learn more about Tensorflow on their site: [**https://www.tensorflow.org/tfx/guide/serving**](https://www.tensorflow.org/tfx/guide/serving)\*\*\*\*

## Quick Start Guide

Running TensorFlow Serving to serve the TensorFlow ResNet model is, as usual, a single line trick.

CPU support:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --cpu --plugin_tfs-resnet
```

GPU support:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --gpu --plugin_tfs-resnet
```

## Test the installation

For a full explanation of how to use Tensorflow Serving please take a look at the documentation site:

### Step 1 - Prepare your client environment

To run any experiment against a remote inference server you have to have tensorflow-serving-api installed on your machine. As per official documentation here:[https://www.tensorflow.org/tfx/serving/setup\#tensorflow\_serving\_python\_api\_pip\_package](https://www.tensorflow.org/tfx/serving/setup#tensorflow_serving_python_api_pip_package)

As reference

```bash
pip install tensorflow-serving-api
```

### Step 2

Clone the TensorFlow repository where we will find the test scripts

```bash
git clone https://github.com/tensorflow/serving
cd serving
```

### Step 3

Find your cluster IP where Tensorflow Serving service is exposed

```bash
kubectl describe service tf-server-service -n tf-serving
```

You should have a similar output:

```bash
Name:                     tf-server-service
Namespace:                tf-serving
Labels:                   <none>
Annotations:              Selector:  app=tf-serv-resnet
Type:                     LoadBalancer
IP:                       10.43.200.139
LoadBalancer Ingress:     172.21.190.98
Port:                     grpc  8500/TCP
TargetPort:               8500/TCP
NodePort:                 grpc  30525/TCP
Endpoints:                10.42.0.139:8500
Port:                     rest  8501/TCP
TargetPort:               8501/TCP
NodePort:                 rest  30907/TCP
Endpoints:                10.42.0.139:8501
Session Affinity:         None
External Traffic Policy:  Cluster
```

Take note of **LoadBalancer Ingress** IP

### Step 4

We can now query the service at its external address from our local host.

Using gRPC:

```bash
python \
  tensorflow_serving/example/resnet_client_grpc.py \
  --server=<LOADBALANCER INGRESS>:8500
```

Using REST Api:

```bash
python \
  tensorflow_serving/example/resnet_client.py \
  --server=<LOADBALANCER INGRESS>:8501
```

You should have an output similar to this:

```bash
#REST Api
Prediction class: 286, avg latency: 87.9074 ms

#gRPC

[INFO]  app=tf-server isn't ready yet. This may take a few minutes...                                    ││ kubeflow     metadata-envoy-deployment-6d776695d9-24xc7       ●  1/1          3 Running      5  11  │

    float_val: 2.1751149688498117e-05
    float_val: 4.679726407630369e-05
    float_val: 6.22767993263551e-06
    float_val: 2.4046405087574385e-05
    float_val: 0.00013994085020385683
    float_val: 5.0004531658487394e-05
    float_val: 1.670094752626028e-05
    float_val: 2.148277962987777e-05
    float_val: 0.0004090495640411973
    float_val: 3.3705742680467665e-05
    float_val: 3.318636345284176e-06
    float_val: 8.649761730339378e-05
    float_val: 3.984206159657333e-06
    float_val: 3.7564968806691468e-06
    float_val: 3.2912407732510474e-06
    float_val: 3.6244309740141034e-06
    float_val: 2.5648103019193513e-06
    float_val: 2.7759107979363762e-05
    float_val: 1.5157910638663452e-05
    float_val: 1.8459862758390955e-06
    float_val: 8.704301990292151e-07
    float_val: 2.724335217862972e-06
    float_val: 3.3186615837621503e-06
    float_val: 1.455540314054815e-06
    float_val: 8.736999006941915e-06
    float_val: 2.299477728229249e-06
    float_val: 2.0985182800359325e-06
    float_val: 0.00026371944113634527
    float_val: 1.0347321222070605e-05
    float_val: 3.660013362605241e-06
    float_val: 2.0003653844469227e-05
    float_val: 6.355750429065665e-06
    float_val: 2.255582785437582e-06
    float_val: 1.5940782986945123e-06
    float_val: 1.2315674666751875e-06
    float_val: 1.1781222610807163e-06
    float_val: 1.4636576452176087e-05
    float_val: 5.812105996483297e-07
    float_val: 6.599811604246497e-05
    float_val: 0.0012952699325978756
  }
}
model_spec {
  name: "resnet"
  version {
    value: 1538687457
  }
  signature_name: "serving_default"
}
```

