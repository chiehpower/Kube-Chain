
```
kubectl cluster-info
```
```

```
Kubernetes master is running at https://127.0.0.1:36777
KubeDNS is running at https://127.0.0.1:36777/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

### Install minikube

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 53.3M  100 53.3M    0     0  10.5M      0  0:00:05  0:00:05 --:--:-- 11.1M
minikube ›› sudo install minikube-linux-amd64 /usr/local/bin/minikube
[sudo] password for chieh: 
minikube ›› sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube ›› ls
minikube-linux-amd64

minikube ›› minikube start
😄  minikube v1.14.1 on Ubuntu 18.04
✨  Automatically selected the docker driver
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.19.2 preload ...
    > preloaded-images-k8s-v6-v1.19.2-docker-overlay2-amd64.tar.lz4: 486.33 MiB
🔥  Creating docker container (CPUs=2, Memory=8000MB) ...
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🔥  Deleting "minikube" in docker ...
🤦  StartHost failed, but will try again: creating host: create: provisioning: ssh command error:
command : sudo hostname minikube && echo "minikube" | sudo tee /etc/hostname
err     : Process exited with status 1
output  : sudo: effective uid is not 0, is /usr/bin/sudo on a file system with the 'nosuid' option set or an NFS file system without root privileges?

🔥  Creating docker container (CPUs=2, Memory=8000MB) ...
😿  Failed to start docker container. Running "minikube delete" may fix it: creating host: create: provisioning: ssh command error:
command : sudo hostname minikube && echo "minikube" | sudo tee /etc/hostname
err     : Process exited with status 1
output  : sudo: effective uid is not 0, is /usr/bin/sudo on a file system with the 'nosuid' option set or an NFS file system without root privileges?

❗  Startup with docker driver failed, trying with alternate driver virtualbox: Failed to start host: creating host: create: provisioning: ssh command error:
command : sudo hostname minikube && echo "minikube" | sudo tee /etc/hostname
err     : Process exited with status 1
output  : sudo: effective uid is not 0, is /usr/bin/sudo on a file system with the 'nosuid' option set or an NFS file system without root privileges?

🔥  Deleting "minikube" in docker ...
🔥  Deleting container "minikube" ...
🔥  Removing /home/chieh/.minikube/machines/minikube ...
💀  Removed all traces of the "minikube" cluster.
💿  Downloading VM boot image ...
    > minikube-v1.14.0.iso.sha256: 65 B / 65 B [-------------] 100.00% ? p/s 0s
    > minikube-v1.14.0.iso: 178.27 MiB / 178.27 MiB [] 100.00% 9.01 MiB p/s 20s
👍  Starting control plane node minikube in cluster minikube
🔥  Creating virtualbox VM (CPUs=2, Memory=6000MB, Disk=20000MB) ...
^C

minikube ›› minikube start
1027 16:07:07.412492  792423 cloud_events.go:60] unable to write to /home/chieh/.minikube/profiles/minikube/events.json: open /home/chieh/.minikube/profiles/minikube/events.json: permission denied
😄  minikube v1.14.1 on Ubuntu 18.04
✨  Using the virtualbox driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🔄  Restarting existing virtualbox VM for "minikube" ...
🐳  Preparing Kubernetes v1.19.2 on Docker 19.03.12 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass, dashboard
🏄  Done! kubectl is now configured to use "minikube" by default
```

### minikube dashboard

```
minikube dashboard
```

Output: 
```
🔌  Enabling dashboard ...
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:36731/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...

(google-chrome-stable:2497204): Gtk-WARNING **: 15:18:35.742: Theme parsing error: gtk.css:6757:29: Missing opening bracket in color definition

(google-chrome-stable:2497204): Gtk-WARNING **: 15:18:35.742: Theme parsing error: gtk.css:6759:34: Missing opening bracket in color definition

(google-chrome-stable:2497204): Gtk-WARNING **: 15:18:35.742: Theme parsing error: gtk.css:6765:28: Missing opening bracket in color definition
Opening in existing browser session.
```

---
# Try on GCP

# Install gcloud tool on local
https://cloud.google.com/sdk/docs/quickstart

(Optional) If you'd like a more streamlined screen reader experience, the gcloud command-line tool comes with an `accessibility/screen_reader` property.

To enable this property, run:
```
gcloud config set accessibility/screen_reader true
```

---
# init gcloud
```
gcp ›› ./google-cloud-sdk/bin/gcloud init                                                                                                 
Welcome! This command will take you through the configuration of gcloud.

Your current configuration has been set to: [default]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.                                                                                                       
Reachability Check passed.
Network diagnostic passed (1/1 checks passed).

You must log in to continue. Would you like to log in (Y/n)?  Y

Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=32555940559.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A8085%2F&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fappengine.admin+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcompute+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Faccounts.reauth&state=aGIgjDBB3PFtg52MA1jywo0MomkeXi&access_type=offline&code_challenge=9wBZNwXBUKm7SN9B2kVW32smfM-vuTOvZb_pqA4-VnE&code_challenge_method=S256


(google-chrome-stable:2121438): Gtk-WARNING **: 09:21:53.557: Theme parsing error: gtk.css:6757:29: Missing opening bracket in color definition

(google-chrome-stable:2121438): Gtk-WARNING **: 09:21:53.557: Theme parsing error: gtk.css:6759:34: Missing opening bracket in color definition

(google-chrome-stable:2121438): Gtk-WARNING **: 09:21:53.557: Theme parsing error: gtk.css:6765:28: Missing opening bracket in color definition
Opening in existing browser session.
You are logged in as: [chiehtsaiphysicist@gmail.com].

Pick cloud project to use: 
 [1] notional-data-(Your project ID)
 [2] Create a new project
Please enter numeric choice or text value (must exactly match list 
item):  [1]
Please enter a value between 1 and 2, or a value present in the list:  1

Your current project has been set to: [notional-data-(Your project ID)].

Not setting default zone/region (this feature makes it easier to use
[gcloud compute] by setting an appropriate default value for the
--zone and --region flag).
See https://cloud.google.com/compute/docs/gcloud-compute section on how to set
default compute region and zone manually. If you would like [gcloud init] to be
able to do this for you the next time you run it, make sure the
Compute Engine API is enabled for your project on the
https://console.developers.google.com/apis page.

Created a default .boto configuration file at [/home/chieh/.boto]. See this file and
[https://cloud.google.com/storage/docs/gsutil/commands/config] for more
information about configuring Google Cloud Storage.
Your Google Cloud SDK is configured and ready to use!

* Commands that require authentication will use chiehtsaiphysicist@gmail.com by default
* Commands will reference project `notional-data-(Your project ID)` by default
Run `gcloud help config` to learn how to change individual settings

This gcloud configuration is called [default]. You can create additional configurations if you work with multiple accounts and/or projects.
Run `gcloud topic configurations` to learn more.

Some things to try next:

* Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
* Run `gcloud topic --help` to learn about advanced features of the SDK like arg files and output formatting
```

---
# Install kubectl via gcloud
```
gcloud components install kubectl
```

### Output
```
Your current Cloud SDK version is: 319.0.0
Installing components from version: 319.0.0

These components will be installed.

Name: kubectl
Version: 1.15.11
Size: 84.1 MiB

Name: kubectl
Version: 1.15.11
Size: < 1 MiB

//  (Skip)

Performing post processing steps...done.                                                                                                  

Update done!

WARNING:   There are other instances of Google Cloud Platform tools on your system PATH.
  Please remove the following to avoid confusion or accidental invocation:

  /usr/bin/snap
/usr/bin/kubectl
```

---
# Set Project and set the zone 
(The default is `us-central1-b`) 
You can use this command to check the zone list. `gcloud compute zones list`

```
gcloud config set project (Your project ID)
gcloud config set compute/zone us-central1-b
export PROJECT_ID="$(gcloud config get-value project -q)"
```
So I set the Asia one
```
gcloud config set compute/zone asia-east1-b
```

---
# Authenticated with the Google Cloud SDK
```
gcloud auth login
```

---
# Try to build an image on GCP 

```
git clone https://github.com/GoogleCloudPlatform/kubernetes-engine-samples
cd kubernetes-engine-samples/hello-app
docker build -t gcr.io/${PROJECT_ID}/hello-app:v1 . 
gcloud builds submit --tag gcr.io/${PROJECT_ID}/hello-app:v1 . 
```
Of course, this image will build on local first.

Test on local first.
```
docker run --rm -p 8080:8080 gcr.io/${PROJECT_ID}/hello-app:v1
```
Then you can switch to Google Cloud Shell to test it.
```
docker run --rm -p 8080:8080 gcr.io/(Your project ID)/hello-app:v1
``` 

**Note**
Actually you can auth first, and then push it.
```
gcloud auth configure-docker
docker push  gcr.io/${PROJECT_ID}/hello-app:v1
```
Up to you.

---
# Create the kubernetes cluster
For default, gcloud cluster will set 3 nodes. Hence, here we set 2 nodes only.
```
gcloud container clusters create loadbalancedcluster --num-nodes=2
```

1. If you meet this error about API, you can check [here](https://console.cloud.google.com/apis?project=notional-data-(Your project ID)) to enable it. 
```
ERROR: (gcloud.container.clusters.create) ResponseError: code=400, message=Failed precondition when calling the ServiceConsumerManager: tenantmanager::185014: Consumer 925984156150 should enable service:container.googleapis.com before generating a service account.
```

Of course, you can type this command below to solve it.
```
gcloud services enable container.googleapis.com
```

2. If you meet his error about zone, you just set a zone.
```
ERROR: (gcloud.container.clusters.create) One of [--zone, --region] must be supplied: Please specify location.
```
Please go back to set the zone, then it can be solved.

Let's try again!
```
hello-app ›› gcloud container clusters create loadbalancedcluster --num-nodes=2                                                               master
WARNING: Warning: basic authentication is deprecated, and will be removed in GKE control plane versions 1.19 and newer. For a list of recommended authentication methods, see: https://cloud.google.com/kubernetes-engine/docs/how-to/api-server-authentication
WARNING: Currently VPC-native is not the default mode during cluster creation. In the future, this will become the default mode and can be disabled using `--no-enable-ip-alias` flag. Use `--[no-]enable-ip-alias` flag to suppress this warning.
WARNING: Newly created clusters and node-pools will have node auto-upgrade enabled by default. This can be disabled using the `--no-enable-autoupgrade` flag.
WARNING: Starting with version 1.18, clusters will have shielded GKE nodes by default.
WARNING: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s). 
WARNING: Starting with version 1.19, newly created clusters and node-pools will have COS_CONTAINERD as the default node image when no image type is specified.
Creating cluster loadbalancedcluster in asia-east1-b... Cluster is being health-checked (master is healthy)...done.                                 
Created [https://container.googleapis.com/v1/projects/(Your project ID)/zones/asia-east1-b/clusters/loadbalancedcluster].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/asia-east1-b/loadbalancedcluster?project=(Your project ID)
kubeconfig entry generated for loadbalancedcluster.
NAME: loadbalancedcluster
LOCATION: asia-east1-b
MASTER_VERSION: 1.16.13-gke.401
MASTER_IP: 35.201.177.220
MACHINE_TYPE: n1-standard-1
NODE_VERSION: 1.16.13-gke.401
NUM_NODES: 2
STATUS: RUNNING
```

After we create the cluster, let's check the node (i.e., VM instances)
```
gcloud compute instances list | grep gke  
```

---
# Deploy the application on GKE cluster

```
hello-app ›› kubectl run web --image=gcr.io/${PROJECT_ID}/hello-app:v1 --port 8080                                                            master
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/web created
```
Check it
```
hello-app ›› kubectl get pods                                                                                                                 master
NAME                  READY   STATUS    RESTARTS   AGE
web-7657d99dc-pzjw9   1/1     Running   0          4m24s
```

---
# Create `Service` and Open `NodePort`

```
hello-app ›› kubectl expose deployment web --target-port=8080 --type=NodePort                                                                 master
service/web exposed

hello-app ›› kubectl get service web                                                                                                          master
NAME   TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
web    NodePort   10.3.246.142   <none>        8080:32438/TCP   3s

```

# Reference
- [ [手把手教學] 如何在 GKE 上部署容器應用程式及 HTTP(S) Load Balancer](https://blog.gcp.expert/tutorials-kubernetes-engine-load-balancer/)
- 