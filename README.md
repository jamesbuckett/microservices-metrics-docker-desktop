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

Please use this repo [Install Kubernetes Tools](https://github.com/jamesbuckett/kubernetes-tools) to install the following on `digital-ocean-droplet`
* kubectl - Interact with Kubernetes cluster
* kubectx - Change clusters
* kubens - Change namespaces like you would directories
* kube-ps1 - Changes Prompt to reflect current cluster and namespace
* helm 3 - Kubernetes package installer  
* kubectl top - Kubernetes top command
* Octant - Kubernetes Web User Interface

## 3. Online Boutique

What is [Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo/)?

Create a Online Boutique namespace: `kubectl create namespace ns-microservices-demo`

Install Online Boutique
```
kubectl apply -n ns-microservices-demo -f "https://raw.githubusercontent.com/jamesbuckett/microservices-metrics-docker-desktop/master/complete-demo.yaml"
```

## 4. Loki

Details on the [Loki Helm Chart](https://grafana.github.io/loki/charts/)

Create a loki namespace: `kubectl create ns ns-loki`

Add the Loki helm repo: `helm repo add loki https://grafana.github.io/loki/charts`

Update the Loki helm repo: `helm repo update`

Install Loki: 
```
helm upgrade --install loki-release loki/loki-stack -f  "https://raw.githubusercontent.com/jamesbuckett/microservices-metrics-docker-desktop/master/values.yaml" -n ns-loki
```

## 5. Grafana

What is [Grafana](https://grafana.com/grafana/)

Start Octant 

Navigate to `ns-loki` namespace

Navigate to Workloads...Pods

Select the `loki-grafan-xxxxxxxxxxxxxxxx` pod

Navigate down to Container grafana

Under `Container Ports` select: Container Ports...`3000/TCP`...`Start Port Forward`

Click on the hyperlink.

Obtain the password: `kubectl get secret --namespace ns-loki loki-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`

Username: `admin`
Password: Value obtained in command above

## 6. Grafana Dashboards

Loki Dashboard
* Import this dashboard: `12019`

Cluster Monitoring for Kubernetes
* Import this dashboard: `10000`

Cluster Monitoring for Kubernetes
* Import this dashbaord: `1471`

## 7. Theory

### 7.1 Loki 

What is [Loki](https://grafana.com/oss/loki/)?

Content derived from [here](https://grafana.com/blog/2020/05/12/an-only-slightly-technical-introduction-to-loki-the-prometheus-inspired-open-source-logging-system/)

Log Analysis Use Cases
* Debugging and troubleshooting
    * Why did my application crash
* Monitoring
    * Create metrics from logs
* Cybersecurity
* Compliance
    * Keep Audit logs
* Business intelligence

Challanges operating traditional Log Management tools
* Hard to operate at scale at petabyte-scale with full-text search index
* Expensive in human, infrastructure and licencing costs
* Does not correlate well with Prometheus metrics

Loki differences
* Loki only indexes a small bit of metadata from every logline.
* Loki data model and query langauge is similar to Prometheus

Log line example from a web server : 
```
10.185.248.71 - - [09/Jan/2015:19:12:06 +0000] 808840 "GET /inventoryService/purchaseItem?userId=20253471&amp;itemId=23434300 HTTP/1.1" 500 17 "-" "Apache-HttpClient/4.2.6 (java 1.5)"
```

Elasticsearch Index
```
Timestamp     = 09/Jan/2015:19:12:06 +0000
Client_ip     = 10.185.248.71
URI           = /inventoryService/purchaseItem
UserId        = 20253471
itemId        = 23434300 
Method        = GET
Size          = 808840
Duration      = 17
HTTPStatus    = 500
Protocol      = HTTP/1.1
Agent         = Apache-HttpClient/4.2.6 (java 1.5)
Msg           = “10.185.248.71 - - [09/Jan/2015:19:12:06 +0000] 808840 "GET /inventoryService/purchaseItem?userId=20253471&amp;itemId=23434300 HTTP/1.1" 500 17 "-" "Apache-HttpClient/4.2.6 (java 1.5)”
```

Loki Index
```
Timestamp     = 09/Jan/2015:19:12:06 +0000
Method        = GET
HTTPStatus    = 500
```

Loki only indexes a *few low cardinality fields upfront*, so the Loki *index is very small*. 

We can query the logs by filtering them by *time range and indexed fields* (called labels in Loki)  and then scanning the remaining set of log lines using substring search or with regular expressions. 

This turns out to be more efficient in the long run and perfectly *suitable for the debug and troubleshooting* use case where you need to find a needle in the proverbial haystack and correlate your Prometheus metrics with your logs.

### 7.1 Loki  Labels 

Content derived from [here](https://grafana.com/blog/2020/04/21/how-labels-in-loki-can-make-log-queries-faster-and-easier/)

Labels are *key value pairs* and can be defined as anything! 

We like to refer to them as metadata to describe a log stream. 

If you are familiar with Prometheus, there are a few labels you are used to seeing like `job` and `instance`, and I will use those in the coming examples.

Labels in Loki perform a very important task. 

They define a stream. 

More specifically, the combination of every label key and value defines the stream.

*End of Section*

