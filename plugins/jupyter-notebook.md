# Jupyter Notebook

Project Jupyter exists to develop open-source software, open-standards, and services for interactive computing across dozens of programming languages. - ****[**https://jupyter.org/**](https://jupyter.org/)\*\*\*\*

We do support the current list of Jupyter Stacks as indicated in:

 [**https://jupyter-docker-stacks.readthedocs.io/**](https://jupyter-docker-stacks.readthedocs.io/)\*\*\*\*

In order to run Jupyter Notebooks just run the following command:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --plugin_jupyter-minimal
```

The following notebooks plugins are available:

* `--plugin_jupyter-minimal`: \(jupyter-minimal\) for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-minimal-notebook)\*\*\*\*
* `--plugin_jupyter-r`:  \(jupyter-r-notebook\) for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-r-notebook)\*\*\*\*
* `--plugin_jupyter-scipy`: \(jupyter-scipy-notebook\) for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-scipy-notebook)\*\*\*\*
* `--plugin_jupyter-tf`:  \(jupyter-tensorflow-notebook\) for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-tensorflow-notebook)\*\*\*\*
* `--plugin_jupyter-datascience`:  \(jupyter-datascience-notebook\) for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-minimal-notebook)\*\*\*\*
* `--plugin_jupyter-pyspark`:  \(jupyter-pyspark-notebook\) for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-pyspark-notebook)\*\*\*\*
* `--plugin_jupyter-allspark`: \(jupyter-all-spark-notebook\)for more information please see [**here**](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook)\*\*\*\*

Note: all Jupyter Notebooks container Kubeflow SDK \(kfp\) library to interact with Kubeflow pipelines. In order to install pipelines and Jupyter at the same time use:

```bash
curl -sfL https://get.k3ai.in | bash -s -- --plugin_jupyter-<YOUR SELECTED FLAVOR> --plugin_kfpipelines 
```

