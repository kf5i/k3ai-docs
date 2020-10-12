# Argo Workflows

## Quick Start Guide

You only have to decide if you want **CPU** support:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --cpu --plugin_argo_workflow
```

or you prefer **GPU** support:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --gpu --plugin_argo_workflow
```

### What is Argo Workflows?

Argo Workflows is an open source container-native workflow engine for orchestrating parallel jobs on Kubernetes. Argo Workflows is implemented as a Kubernetes CRD.

* Define workflows where each step in the workflow is a container.
* Model multi-step workflows as a sequence of tasks or capture the dependencies between tasks using a graph \(DAG\).
* Easily run compute intensive jobs for machine learning or data processing in a fraction of the time using Argo Workflows on Kubernetes.
* Run CI/CD pipelines natively on Kubernetes without configuring complex software development products.

Learn more on the Kubeflow website: [**https://argoproj.github.io/projects/argo**](https://argoproj.github.io/projects/argo)\*\*\*\*

