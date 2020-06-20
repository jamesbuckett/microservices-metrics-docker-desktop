# A Tutorial on Microservices and Observability on Docker for Desktop 

![image](https://user-images.githubusercontent.com/18049790/43352583-0b37edda-9269-11e8-9695-1e8de81acb76.png)

## Disclaimer
* Opinions expressed here are solely my own and do not express the views or opinions of JPMorgan Chase.
* Any third-party trademarks are the intellectual property of their respective owners and any mention herein is for referential purposes only. 

## Table of Contents

1. Introduction
* 1.1 Agenda
* 1.2 Requirements
2. Docker Desktop 
* 2.1 What is Digital Ocean?
3. Online Boutique (Micro-service)
* 3.1 What is Online Boutique?
* 3.2 Install Online Boutique
4. Helm (Package Manager)
* 4.1 What is Helm?
* 4.2 Install Helm 3
5. Install Loki (Metrics UI)
* 4.1 What is Grafana?
* 4.2 Access the Grafana UI
* 4.3 Observing Online Boutique with Grafana
6. Practical - The Fun Starts Here
* 6.1 Start User Interfaces
*   8.1.1 Locust
9. Tutorial Clean Up
* 9.1 CLI Method
* 9.2 GUI Method
10. Theory
* 11.1 Documentation
* 11.2 Buzz Words

## 1. Introduction

### 1.1 Agenda
* Deploy a Ubuntu jump host on Digital Ocean with SSH access
* Deploy a Kubernetes cluster on Digital Ocean with Observability software pre-configured
* Deploy Loki for distributed logging on the cluster
* Deploy the Online Boutique micro-services application onto the Kubernetes cluster on Digital Ocean
* Verify operation of the Online Boutique micro-service
* Observe the Online Boutique micro-service with the Observability software
* Perform Chaos Engineering on the Online Boutique micro-service

### 1.2 Requirements
* 1.2.1 A Digital Ocean Account
  * A credit card or debit card is required to sign up to Digital Ocean
  * The Referral Link provided gives $100 credit for 60 days to offset the cost of this tutorial 
* 1.2.2 A Terminal Emulator to interact with the cluster
  * If using Windows 10 please install the following software:
    * [PuTTY](https://www.putty.org/) 
    * [PuTTYgen](https://www.puttygen.com/)
    * [WinSCP](https://winscp.net/eng/download.php)
  * Mac  
    * [Terminal on Mac](https://support.apple.com/en-sg/guide/terminal/welcome/mac)

### 1.3 Cost Warning
Note: This stack requires a minimum configuration of
* 3 * Kubernetes Nodes at $10/month (2GB memory / 1 vCPU) 
* 2 * Network Load Balancers at $10/month 
* 1 * Ubuntu Droplet at $5/month
* **Total cost of $55 per month if kept running**

```diff
- Please tear all infrastructure at the end of this tutorial or you will incur a cost at the end of the month -
- I accept no liability for any costs incurred - 
```

## 2. Digital Ocean - Cloud Provider
