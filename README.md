# Kube-Chain
This repo will record some notes and experiments about kube-chain tools including kubernetes, kubeflow, kfserving and so on.

**The purpose is to understand how to use k8s, and the final goal is to deploy kubeflow and successfully implement MLOps.**

*The notes were quite messy.*

>My environment is Ubuntu 18.04.

# Install Kubernetes

## Install kubectl

- Check here : [Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

First, we are going to install `kubectl` which is Kubernetes command-line tool and it allows you to run commands against Kubernetes clusters.

Install kubectl binary with curl on Linux 

```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" 
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

My output:
>Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.2", GitCommit:"f5743093fd1c663cb0cbc89748f730662345d44d", GitTreeState:"clean", BuildDate:"2020-09-16T13:41:02Z", GoVersion:"go1.15", Compiler:"gc", Platform:"linux/amd64"}

## Install kubeadm kubelet kubectl

```
# (Cannot work) sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt-get install kubeadm kubelet kubectl -y
```

Common tools of k8s:
Generally, k8s has 3 tools that can be used on the master node:

1. kubectl
2. kubeadm
3. kubelet

Check the k8s version:
```
kubeadm version
```

Output:
```
kubeadm version: &version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.2", GitCommit:"f5743093fd1c663cb0cbc89748f730662345d44d", GitTreeState:"clean", BuildDate:"2020-09-16T13:38:53Z", GoVersion:"go1.15", Compiler:"gc", Platform:"linux/amd64"}
```
## Install k8s

From `https://dl.k8s.io/v1.19.0/kubernetes.tar.gz`

extract it. 

## Install Kind
From `https://kind.sigs.k8s.io/docs/user/quick-start/`

```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64
chmod +x ./kind
mv ./kind /usr/bin/kind 
```

k8s has 2 servers. One is client, and another one is server.

## Create a cluster

```
$ kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.19.1) 🖼 
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
```

Let's check the cluster info.
```
$ kubectl cluster-info --context kind-kind

Kubernetes master is running at https://127.0.0.1:36777
KubeDNS is running at https://127.0.0.1:36777/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

# Deploy the NVIDIA device plugin

```
Command:
kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/1.0.0-beta4/nvidia-device-plugin.yml

Output:
daemonset.apps/nvidia-device-plugin-daemonset created
```

---

```
apiVersion: v1
kind: Pod
metadata:
  name: cuda-vector-add
spec:
  restartPolicy: OnFailure
  containers:
    - name: cuda-vector-add
      # https://github.com/kubernetes/kubernetes/blob/v1.7.11/test/images/nvidia-cuda/Dockerfile
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1 # requesting 1 GPU
```