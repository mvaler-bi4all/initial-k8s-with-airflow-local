# Local running Airflow with K8s

## Pre requisites
Kubectl,Helm,Docker,KinD

## Create a kubernetes cluster - 1 control plane and 3 worker nodes
kind create cluster --name airflow-cluster --config kind-cluster.yaml

## Add the official repository of the Airflow Helm Chart
helm repo add apache-airflow https://airflow.apache.org

## Update the repo
helm repo update

## Create namespace airflow
kubectl create namespace airflow

## Install the Airflow Helm Chart
helm install airflow apache-airflow/airflow --namespace airflow --debug —-timeout 10m0s

## Port forward 8080:8080
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow --context kind-airflow-cluster


## To update Airflow deployment

## Check PODs
kubectl get pods -n airflow

### Check the current revision
helm ls -n airflow

## Upgrade the chart
helm upgrade --install airflow apache-airflow/airflow -n airflow -f values.yaml --debug

## Check the nodes with kubectl
kubectl get nodes -o wide

## Check the namespace
kubectl get namespaces
