# KuBeChain
[![](https://img.shields.io/badge/Status-Updating-blue)](./)

KuBeChain recorded some my notes and experiments about kube-chain tools including kubernetes, kubeflow, kfserving and so on.

**The purpose is to understand how to install the Kubernetes, and the final goal is to deploy the kubeflow and successfully implement MLOps.**

*The notes were a little bit messy until I fully implemented it.*

>My testing environments are Ubuntu v18.04 and v20.04.

## Introduction

Let me briefly explain for someone who wanna learn and try kubernetes and their chain.

Basically there are some ways to create a cluster via different tools or platforms that it fully depends on your purpose.

**Cluster is Cluster which can be created by many tools.**

When I tried to install k8s at the first time, I just thought "ok it should be only one tool to be able to install k8s.", but obviously it was not. It is independent. For example, we can imagine that k8s is like a city, and we can take a bus or drive a car to there. There are some transports to bring us to there, so that means we have many choices to achieve k8s deployment. However, for the beginner, it will make a little bit confused because we do not know which one is more suitable for us to apply and practice. Each tool has its commands and its behavior. For instance, if you choose the car, you have to understand how to drive the car and how to control it, right? So it is one of difficult levels for beginners to familiarize and understand k8s behavior. 

> Tools : microk8s, minikube, some clould platforms (e.g., AWS, GCP, Azure), etc
>
> The situations can be categorized such as single node (single machine), local or cloud service, container or VM. 

Creating a cluster is the initial step. Sequentially, there is a specific tool to control k8s, kubectl. We will use the kubectl tool to control the settings of k8s. In that part, I highly recommend you have to get some experience on docker. Otherwise, it will be too hard to handle.

> Prerequisites: docker background knowledge
>
> Tools : kubectl

Next part, it is about the deploying kubeflow. As we know that the kubeflow will connect to a lot of components, mostly it would get some troubles on enabling / applying the kubeflow entire compoments. 



Update soon...