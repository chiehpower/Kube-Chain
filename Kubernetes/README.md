
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