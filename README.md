# PySpark Demo with Jupyter Notebook on Kubernetes
## Introduction
This is a demo of PySpark with Jupyter Notebook on Kubernetes. It is based on the [official PySpark Docker image](https://hub.docker.com/r/jupyter/pyspark-notebook/).

## Prerequisites
* [Kubernetes](https://kubernetes.io/)
* Spark 3.4.1

## Installation
### Kubernetes
Install Kubernetes on your machine. You can use:
* [Docker Desktop](https://docs.docker.com/desktop/) with Kubernetes cluster enabled
* [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
* Any other Kubernetes cluster

### Spark
If you have wget and tar command available, you can use the following commands:
```bash
wget https://archive.apache.org/dist/spark/spark-3.4.1/spark-3.4.1-bin-hadoop3.tgz
tar -xzf spark-3.4.1-bin-hadoop3.tgz
```
Otherwise, you can do it manually - download Spark 3.4.1 from [here](https://spark.apache.org/downloads.html) and unpack the archive to `spark-3.4.1-bin-hadoop3/`.

### Spark Docker image
Build the Spark Docker image for Kubernetes and PySpark. You can use the following command:
```bash
cd ./spark-3.4.1-bin-hadoop3.2/
bin/docker-image-tool.sh -r docker.io/yourreponame -t v3.4.1 -p kubernetes/dockerfiles/spark/bindings/python/Dockerfile build
```

## Deploying Jupyter Notebook on Kubernetes
Before deploying the Jupyter Notebook with PySpark on Kubernetes, you need to update mount path in deployment.yaml file. You can do it by replacing `<path_to_project>` in `/<path_to_project>/pyspark-kuberentes/pyspark-notebook` with the real path to your project. Ensure that the path is absolute.

Once it's done, you can deploy the Jupyter Notebook with PySpark on Kubernetes using the following command:
```bash
kubectl apply -f ./kubernetes
```

## Accessing Jupyter Notebook
To access the Jupyter Notebook, you need to get the URL of the service. You can do it by running the following command:
```bash
kubectl get pods -n spark
kubectl -n spark logs <pyspark-notebook-pod-name>
```
In the logs you will find the proper url with token allowing you to access the web interface.

You will see Jupyter notebook with the `work` directory containing example code that will run Spark workers and make some computations.

Have fun!
