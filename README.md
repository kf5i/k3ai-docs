# K3ai \(keɪ3ai\)

K3ai is a lightweight infrastructure-in-a-box specifically built to install and configure AI tools and platforms to quickly experiment and/or run in production over edge devices.

![](.gitbook/assets/aio.gif)

## Ready to experiment?

```text
curl -sfL https://get.k3ai.in | bash -
```

Looking for more interaction? join our Slack channel [**here**](https://join.slack.com/t/k3ai/shared_invite/zt-j61vfvkx-tCD~k9l2218lu7ZplRLGNA)\*\*\*\*

## Components of K3ai

Currently, we install the following components \(the list is changing and growing\):

* Kubernetes based on K3s from Rancher: [https://k3s.io/](https://k3s.io/)
* Kubeflow pipelines: [https://github.com/kubeflow/pipelines](https://github.com/kubeflow/pipelines)
* Argo Workflows: [https://github.com/argoproj/argo](https://github.com/argoproj/argo)
* Kubeflow: [https://www.kubeflow.org/](https://www.kubeflow.org/) - **\(coming soon\)**
* NVIDIA GPU support: [https://docs.nvidia.com/datacenter/cloud-native/index.html](https://docs.nvidia.com/datacenter/cloud-native/index.html)
* NVIDIA Triton inference server: [https://github.com/triton-inference-server/server/tree/master/deploy/single\_server](https://github.com/triton-inference-server/server/tree/master/deploy/single_server) **\(coming soon\)**
* Tensorflow Serving: [https://www.tensorflow.org/tfx/serving/serving\_kubernetes](https://www.tensorflow.org/tfx/serving/serving_kubernetes):
  * ResNet
  * Mnist **\(coming soon\)**

