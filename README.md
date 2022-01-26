# Local running Airflow with k8s

## Pre requisites
Kubectl
Helm
Docker
KinD

## Create a kubernetes cluster - 1 control plane and 3 worker nodes
kind create cluster --name airflow-cluster --config kind-cluster.yaml

## Check the cluster info
kubectl cluster-info

## Check the nodes with kubectl
kubectl get nodes -o wide

## Add the official repository of the Airflow Helm Chart
helm repo add apache-airflow <https://airflow.apache.org>

## Update the repo
helm repo update

## Create namespace airflow
kubectl create namespace airflow

## Check the namespace
kubectl get namespaces

## Install the Airflow Helm Chart
helm install airflow apache-airflow/airflow --namespace airflow --debug —-timeout 10m0s

## Get pods
kubectl get pods -n airflow

## Check release
helm ls -n airflow

## Port forward 8080:8080
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow --context kind-airflow-cluster

## To update Airflow deployment
## Check the current revision
helm ls -n airflow

## Upgrade the chart
helm upgrade --install airflow apache-airflow/airflow -n airflow -f values.yaml --debug

## Check after
helm ls -n airflow
