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





## 2. Digital Ocean - Cloud Provider
