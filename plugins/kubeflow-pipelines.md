# Kubeflow Pipelines

Kubeflow Pipelines is a platform for building and deploying portable, scalable machine learning \(ML\) workflows based on Docker containers.

## Quick Start Guide

You only have to decide if you want **CPU** support:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --cpu --plugin_kfpipelines
```

or if you prefer **GPU** support:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --gpu --plugin_kfpipelines
```

## What is Kubeflow Pipelines? <a id="what-is-kubeflow-pipelines"></a>

The Kubeflow Pipelines platform consists of:

* A user interface \(UI\) for managing and tracking experiments, jobs, and runs.
* An engine for scheduling multi-step ML workflows.
* An SDK for defining and manipulating pipelines and components.
* Notebooks for interacting with the system using an SDK.

The following are the goals of Kubeflow Pipelines:

* End-to-end orchestration: enabling and simplifying the orchestration of machine learning pipelines.
* Easy experimentation: making it easy for you to try numerous ideas and techniques and manage your various trials/experiments.
* Easy re-use: enabling you to re-use components and pipelines to quickly create end-to-end solutions without having to rebuild each time.

Learn more on the Kubeflow website: [**https://www.kubeflow.org/docs/pipelines/**](https://www.kubeflow.org/docs/pipelines/)\*\*\*\*

