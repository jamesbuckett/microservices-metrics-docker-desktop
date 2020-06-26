# A Tutorial on Microservices and Observability on Docker for Desktop 

```diff
- This is a work in progress and not complete -
```

![image](https://user-images.githubusercontent.com/18049790/43352583-0b37edda-9269-11e8-9695-1e8de81acb76.png)

## Disclaimer
* Opinions expressed here are solely my own and do not express the views or opinions of JPMorgan Chase.
* Any third-party trademarks are the intellectual property of their respective owners and any mention herein is for referential purposes only. 

## Table of Contents

1. Introduction
* 1.1 Agenda
* 1.2 Requirements

## 1. Introduction

### 1.1 Agenda
* Install Helm
* Install Octant
* Install Online Boutique
* Install Loki
* Check Online Boutique logs with Loki
* Check Online Boutique metrics with Prometheus


### 1.2 Requirements
* A terminal shell 
* Docker for Desktop installed and working

## 2. Install Tools 

### 2.1 Helm 

```
cd ~/ && mkdir helm-3 && cd helm-3
wget https://get.helm.sh/helm-v3.2.1-linux-amd64.tar.gz
tar -zxvf helm-v3.2.1-linux-amd64.tar.gz
mv linux-amd64/helm /usr/local/bin/helm
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

### 2.2 Octant 

Install Octant as per your [platform](https://github.com/vmware-tanzu/octant).

## 3. Online Boutique

Create a Online Boutique namespace: `kubectl create namespace microservices-demo`

Install Online Boutique
```
kubectl apply -n microservices-demo -f "https://raw.githubusercontent.com/jamesbuckett/microservices-metrics-docker-desktop/master/complete-demo.yaml"
```

## 4. Loki

Create a loki namespace: `kubectl create ns loki`

Add the Loki helm repo: `helm repo add loki https://grafana.github.io/loki/charts`

Update the Loki helm repo: `helm repo update`

Install Loki: 
```
helm upgrade --install loki loki/loki-stack -f  "https://raw.githubusercontent.com/jamesbuckett/microservices-metrics-docker-desktop/master/values.yaml" -n loki
```

## 5. Grafana

Start Octant 

Navigate to `loki` namespace

Navigate to Workloads...Pods

Select the `loki-grafan-xxxxxxxxxxxxxxxx` pod

Navigate down to Container grafana

Under `Container Ports` select: Container Ports...`3000/TCP`...`Start Port Forward`

Click on the hyperlink.

Obtain the password: `kubectl get secret --namespace loki loki-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`

Username: `admin`
Password: Value obtained in command above

## 6. Dashboards

Import this dashboard: `12019`

Import this dashboard: `10000`

Import this dashbaord: `1471`


*End of Section*

