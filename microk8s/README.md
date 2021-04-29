My environment is Ubuntu 20.04.
# Try to use `microk8s` to create the k8s

Check the version:

```
latest/stable     snap install --stable microk8s
latest/candidate  snap install --candidate microk8s
latest/beta       snap install --beta microk8s
1.21/stable       snap install --channel=1.21 microk8s
1.21/candidate    snap install --channel=1.21/candidate microk8s
1.21/beta         snap install --channel=1.21/beta microk8s
1.20/stable       snap install --channel=1.20 microk8s
1.20/candidate    snap install --channel=1.20/candidate microk8s
1.20/beta         snap install --channel=1.20/beta microk8s
1.19/stable       snap install --channel=1.19 microk8s
1.19/candidate    snap install --channel=1.19/candidate microk8s
1.19/beta         snap install --channel=1.19/beta microk8s
1.18/stable       snap install --channel=1.18 microk8s
1.18/candidate    snap install --channel=1.18/candidate microk8s
1.18/beta         snap install --channel=1.18/beta microk8s
1.17/stable       snap install --channel=1.17 microk8s
1.17/candidate    snap install --channel=1.17/candidate microk8s
1.17/beta         snap install --channel=1.17/beta microk8s
1.16/stable       snap install --channel=1.16 microk8s
1.16/candidate    snap install --channel=1.16/candidate microk8s
1.16/beta         snap install --channel=1.16/beta microk8s
1.15/stable       snap install --channel=1.15 microk8s
1.15/candidate    snap install --channel=1.15/candidate microk8s
1.15/beta         snap install --channel=1.15/beta microk8s
1.14/stable       snap install --channel=1.14 microk8s
1.14/candidate    snap install --channel=1.14/candidate microk8s
1.14/beta         snap install --channel=1.14/beta microk8s
1.13/stable       snap install --channel=1.13 microk8s
1.13/candidate    snap install --channel=1.13/candidate microk8s
1.13/beta         snap install --channel=1.13/beta microk8s
1.12/stable       snap install --channel=1.12 microk8s
1.12/candidate    snap install --channel=1.12/candidate microk8s
1.12/beta         snap install --channel=1.12/beta microk8s
1.11/stable       snap install --channel=1.11 microk8s
1.11/candidate    snap install --channel=1.11/candidate microk8s
1.11/beta         snap install --channel=1.11/beta microk8s
1.10/stable       snap install --channel=1.10 microk8s
1.10/candidate    snap install --channel=1.10/candidate microk8s
1.10/beta         snap install --channel=1.10/beta microk8s
```

Choose v1.18 to install.
```
$ sudo snap install microk8s --channel=1.18 --classic
```

Check the status:
```
$ sudo microk8s.status
microk8s is not running. Use microk8s inspect for a deeper inspection.
```
Please wait for a while that it will take some time on starting a cluster setting.

Check again after a while.
```
$ sudo microk8s.status 

microk8s is running
addons:
cilium: disabled
dashboard: disabled
dns: disabled
fluentd: disabled
gpu: disabled
helm: disabled
helm3: disabled
ingress: disabled
istio: disabled
jaeger: disabled
knative: disabled
kubeflow: disabled
linkerd: disabled
metallb: disabled
metrics-server: disabled
prometheus: disabled
rbac: disabled
registry: disabled
storage: disabled
```

Enable some relevant tools
```
$ microk8s enable dashboard dns registry istio 

Applying manifest
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created
service/monitoring-grafana created
service/monitoring-influxdb created
service/heapster created
deployment.apps/monitoring-influxdb-grafana-v4 created
serviceaccount/heapster created
clusterrolebinding.rbac.authorization.k8s.io/heapster created
configmap/heapster-config created
configmap/eventer-config created
deployment.apps/heapster-v1.5.2 created

If RBAC is not enabled access the dashboard using the default token retrieved with:

token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s kubectl -n kube-system describe secret $token

In an RBAC enabled setup (microk8s enable RBAC) you need to create a user with restricted
permissions as shown in:
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

dns is already enabled
Enabling the private registry
storage is already enabled
Applying registry manifest
namespace/container-registry created
persistentvolumeclaim/registry-claim created
deployment.apps/registry created
service/registry created
The registry is enabled
Enabling Istio
Fetching istioctl version v1.3.4.
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   630  100   630    0     0     63      0  0:00:10  0:00:09  0:00:01   135
100 36.3M  100 36.3M    0     0  2218k      0  0:00:16  0:00:16 --:--:-- 5133k

(skip some processes...)
```

In the process, it will ask this question:
```
Enforce mutual TLS authentication (https://bit.ly/2KB4j04) between sidecars? If unsure, choose N. (y/N):
```
I chose "y".

In the end, the output message will be like below:
```
instance.config.istio.io/tcpbytereceived created
instance.config.istio.io/tcpconnectionsopened created
instance.config.istio.io/tcpconnectionsclosed created
handler.config.istio.io/prometheus created
rule.config.istio.io/promhttp created
rule.config.istio.io/promtcp created
rule.config.istio.io/promtcpconnectionopen created
rule.config.istio.io/promtcpconnectionclosed created
handler.config.istio.io/kubernetesenv created
rule.config.istio.io/kubeattrgenrulerule created
rule.config.istio.io/tcpkubeattrgenrulerule created
instance.config.istio.io/attributes created
destinationrule.networking.istio.io/istio-policy created
destinationrule.networking.istio.io/istio-telemetry created
Istio is starting
```

Then let use check the dashboard.
```
$ microk8s dashboard-proxy        
Checking if Dashboard is running.
Waiting for Dashboard to come up.
Dashboard will be available at https://127.0.0.1:10443
Use the following token to login:
```

Access this link and choose to login by token.
Then you can see the dashboard.
![](./assets/dashboard.png)

---
### Install kubeflow

```
$ microk8s enable kubeflow

Enabling dns...
Enabling storage...
Enabling ingress...
Enabling metallb:10.64.140.43-10.64.140.49...
Waiting for other addons to finish initializing...
Addon setup complete. Checking connectivity...
Bootstrapping...
Bootstrap complete.
Successfully bootstrapped, deploying...
Kubeflow deployed.
Waiting for operator pods to become ready.
Waited 0s for operator pods to come up, 31 remaining.
Waited 15s for operator pods to come up, 29 remaining.
Waited 30s for operator pods to come up, 29 remaining.
Waited 45s for operator pods to come up, 27 remaining.
Waited 60s for operator pods to come up, 26 remaining.
Waited 75s for operator pods to come up, 26 remaining.
Waited 90s for operator pods to come up, 25 remaining.
Waited 105s for operator pods to come up, 23 remaining.
Waited 120s for operator pods to come up, 21 remaining.
Waited 135s for operator pods to come up, 20 remaining.
Waited 150s for operator pods to come up, 19 remaining.
Waited 165s for operator pods to come up, 17 remaining.
Waited 180s for operator pods to come up, 14 remaining.
Waited 195s for operator pods to come up, 14 remaining.
Waited 210s for operator pods to come up, 14 remaining.
Waited 225s for operator pods to come up, 12 remaining.
Waited 240s for operator pods to come up, 11 remaining.
Waited 255s for operator pods to come up, 8 remaining.
Waited 270s for operator pods to come up, 7 remaining.
Waited 285s for operator pods to come up, 6 remaining.
Waited 300s for operator pods to come up, 4 remaining.
Waited 315s for operator pods to come up, 2 remaining.
Operator pods ready.

Congratulations, Kubeflow is now available.

The dashboard is available at http://10.64.140.44.xip.io

    Username: admin
    Password: UAZ86H2EVMF1BT1

To see these values again, run:

    microk8s juju config dex-auth static-username
    microk8s juju config dex-auth static-password

To tear down Kubeflow and associated infrastructure, run:

    microk8s disable kubeflow
```


This one is optional.
```
$ microk8s enable gpu
```

Check with the kubectl

```
 microk8s.kubectl get po -n kubeflow 
NAME                                           READY   STATUS    RESTARTS   AGE
admission-webhook-5749db547f-7djp6             1/1     Running   2          100m
admission-webhook-operator-0                   1/1     Running   2          100m
argo-controller-6fd6865897-zzk77               1/1     Running   2          95m
argo-controller-operator-0                     1/1     Running   2          99m
argo-ui-6f9fc485f-hxtzb                        1/1     Running   2          99m
argo-ui-operator-0                             1/1     Running   2          99m
dex-auth-845b474467-z8ns9                      2/2     Running   9          93m
dex-auth-operator-0                            1/1     Running   2          99m
istio-ingressgateway-585b4997ff-wx2tb          1/1     Running   2          93m
istio-ingressgateway-operator-0                1/1     Running   2          99m
istio-pilot-6fb7678498-mg4b5                   1/1     Running   2          98m
istio-pilot-operator-0                         1/1     Running   2          98m
jupyter-controller-78b6b6f97-qmknm             1/1     Running   2          98m
jupyter-controller-operator-0                  1/1     Running   2          98m
jupyter-web-854776b5b-xfv76                    1/1     Running   2          98m
jupyter-web-operator-0                         1/1     Running   2          98m
katib-controller-7599cd86f9-bmq9q              1/1     Running   2          97m
katib-controller-operator-0                    1/1     Running   2          97m
katib-db-0                                     1/1     Running   2          97m
katib-db-operator-0                            1/1     Running   2          97m
katib-manager-7b7b4f8547-9jlql                 1/1     Running   2          97m
katib-manager-operator-0                       1/1     Running   2          97m
katib-ui-55966dfd78-nthd8                      1/1     Running   2          97m
katib-ui-operator-0                            1/1     Running   2          97m
kubeflow-dashboard-58b859f69b-lv5ll            1/1     Running   2          97m
kubeflow-dashboard-operator-0                  1/1     Running   2          97m
kubeflow-profiles-7dcd6997ff-n5jpx             2/2     Running   4          97m
kubeflow-profiles-operator-0                   1/1     Running   2          98m
metacontroller-6b4cd56645-hd7gd                1/1     Running   2          96m
metacontroller-operator-0                      1/1     Running   2          96m
metadata-api-757b78f955-bxq5m                  1/1     Running   4          96m
metadata-api-operator-0                        1/1     Running   2          96m
metadata-db-0                                  1/1     Running   2          96m
metadata-db-operator-0                         1/1     Running   2          96m
metadata-envoy-5d79788846-hhjmq                1/1     Running   2          96m
metadata-envoy-operator-0                      1/1     Running   2          96m
metadata-grpc-5cfc4877c7-c5wqz                 1/1     Running   5          96m
metadata-grpc-operator-0                       1/1     Running   2          96m
metadata-ui-7f86857955-tv7dj                   1/1     Running   2          96m
metadata-ui-operator-0                         1/1     Running   2          96m
minio-0                                        1/1     Running   2          95m
minio-operator-0                               1/1     Running   2          95m
modeloperator-7459469cdd-tzkbc                 1/1     Running   2          100m
oidc-gatekeeper-648d7644d7-mx6mc               2/2     Running   4          93m
oidc-gatekeeper-operator-0                     1/1     Running   2          95m
pipelines-api-76d455df4c-sx4bl                 1/1     Running   4          93m
pipelines-api-operator-0                       1/1     Running   2          94m
pipelines-db-0                                 1/1     Running   2          94m
pipelines-db-operator-0                        1/1     Running   2          95m
pipelines-persistence-569fc867c6-bl4lx         1/1     Running   2          94m
pipelines-persistence-operator-0               1/1     Running   2          94m
pipelines-scheduledworkflow-749bbdcfdd-dx8fq   1/1     Running   2          94m
pipelines-scheduledworkflow-operator-0         1/1     Running   2          94m
pipelines-ui-5ccbcc74cc-57kgw                  1/1     Running   2          94m
pipelines-ui-operator-0                        1/1     Running   2          94m
pipelines-viewer-f57579f47-hqf8v               1/1     Running   2          94m
pipelines-viewer-operator-0                    1/1     Running   2          94m
pipelines-visualization-6bf774fb68-tg6bv       1/1     Running   2          95m
pipelines-visualization-operator-0             1/1     Running   2          95m
pytorch-operator-7b45bb58d4-fzgcf              1/1     Running   3          95m
pytorch-operator-operator-0                    1/1     Running   2          95m
seldon-core-56bbd6fc9b-d8qnv                   1/1     Running   3          95m
seldon-core-operator-0                         1/1     Running   2          95m
tf-job-operator-bbb567995-4k6r9                1/1     Running   3          94m
tf-job-operator-operator-0                     1/1     Running   2          94m
```


---
# Start to practice k8s with microk8s

> Followed the tutorial steps from [[Day5]在Minikube上跑起你的Docker Containers-Pod & kubectl常用指令](https://ithelp.ithome.com.tw/articles/10193232)

A mojority of concepts and steps are similar, but the most different is that I used the `microk8s` instead of using `minikube` in this practice.
Therefore, there are few commands which are different from that tutorials.

**The purpose is to build an image and create a container on k8s.**

Here are some conclusions about basic k8s usages.
The overview workflow is :
    1. Create a pod by a yaml file (This `yaml` file includes the basic your container information setting.)
    2. Get pods to check the status of pods
    3. If your pod get stuck on `ContainerCreating`, use `kubectl describe` to check the status and detail info.
    4. Use the pod by `kubectl port-forward` or `kubectl expose pod` with `kubectl get services` to check how many services.

Notes:
    1. The `spec.container.image` in the `yaml` basically is the path of Docker Hub registry.
    2. The pod name cannot use `_` but it can accept `-`.


### Build a ymal file

Create a yaml file named : `my_app_1.yaml`

```
$ cat my_app_1.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: webserver
spec:
  containers:
  - name: pod-demo
    image: zxcvbnius/docker-demo
    ports:
    - containerPort: 3000

$ microk8s.kubectl create -f my_app_1.yaml
pod/my-pod created
```
If you are not sure your apiVersion version, you can check it by `microk8s.config`

It will take some time on creating the pod. You can also check on the dashboard by `microk8s dashboard-proxy`.

### Check the pod status:

```
$ microk8s.kubectl get pod                                                                                                 solomon@10
NAME     READY   STATUS    RESTARTS   AGE
my-pod   1/1     Running   0          6m
```

Or you can also use `describe` to check more detail info.
```
$ microk8s.kubectl describe pod my-pod

Name:         my-pod
Namespace:    default
Priority:     0
Node:         10.1./10.1.
Start Time:   - 
Labels:       app=webserver
Annotations:  <none>
Status:       Running
IP:           10.1.50.45
IPs:
  IP:  10.1.50.45
Containers:
  pod-demo:
    Container ID:   containerd://54ac1ad509a68d
    Image:          zxcvbnius/docker-demo
    Image ID:       docker.io/zxcvbnius/docker-demo@sha256:a3c6b
    Port:           3000/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 29 Apr 2021 10:21:20 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-m4qck (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-m4qck:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-m4qck
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  6m44s  default-scheduler  Successfully assigned default/my-pod to 10.1.
  Normal  Pulling    6m44s  kubelet            Pulling image "zxcvbnius/docker-demo"
  Normal  Pulled     27s    kubelet            Successfully pulled image "zxcvbnius/docker-demo"
  Normal  Created    27s    kubelet            Created container pod-demo
  Normal  Started    26s    kubelet            Started container pod-demo
-------------------------------------------------------------------------------------------
```

Reference:
- [Kubernetes stuck on ContainerCreating](https://serverfault.com/questions/728727/kubernetes-stuck-on-containercreating)
### Visit our pod

1. Forward the port number from pod to host
    ```
    $ microk8s.kubectl port-forward my-pod 8888:3000
    Forwarding from 127.0.0.1:8888 -> 3000
    Forwarding from [::1]:8888 -> 3000
    ```

    This one can let your host 8888 port number to connect to 3000 port number of the pod. 

    So open our browser to go to this url `http://127.0.0.1:8888` and then we can see the words of hello world!
    
    However, this one you have to keep your terminal. If you close it, then you are not able to access this link. 
2. Create a service
    We use `microk8s.expose` to mapping the port of pod and port of k8s cluster. 
    ```
    $ microk8s.kubectl expose pod my-pod --type=NodePort --name=my-pod-service
    service/my-pod-service exposed
    ```
    Check our services
    ```
    $ microk8s.kubectl get services

    NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
    kubernetes       ClusterIP   10.152.183.1     <none>        443/TCP          139m
    my-pod-service   NodePort    10.152.183.205   <none>        3000:32516/TCP   2m7s
    ```

    Then we can check this link `http://10.152.183.205:3000/` on browser.

3. We can add a label for this pod.
    ```
    $ microk8s.kubectl get pods  --show-labels 
    NAME     READY   STATUS    RESTARTS   AGE   LABELS
    my-pod   1/1     Running   0          67m   app=webserver
    
    $ microk8s.kubectl label pods my-pod version=latest 
    pod/my-pod labeled

    $ microk8s.kubectl get pods  --show-labels
    NAME     READY   STATUS    RESTARTS   AGE   LABELS
    my-pod   1/1     Running   0          69m   app=webserver,version=latest
    ```
4. Use another pod to check this pod.
   First of all, we need to install curl tool. Then use `curl` to check it.
    ```
    $ microk8s.kubectl run -i --tty alpine --image=alpine --restart=Never -- sh 
    If you don't see a command prompt, try pressing enter.
    / # apk add --no-cache curl
    fetch https://dl-cdn.alpinelinux.org/alpine/v3.13/main/x86_64/APKINDEX.tar.gz
    fetch https://dl-cdn.alpinelinux.org/alpine/v3.13/community/x86_64/APKINDEX.tar.gz
    (1/5) Installing ca-certificates (20191127-r5)
    (2/5) Installing brotli-libs (1.0.9-r3)
    (3/5) Installing nghttp2-libs (1.42.0-r1)
    (4/5) Installing libcurl (7.76.1-r0)
    (5/5) Installing curl (7.76.1-r0)
    Executing busybox-1.32.1-r6.trigger
    Executing ca-certificates-20191127-r5.trigger
    OK: 8 MiB in 19 packages
    / # curl http://10.1.50.45:3000
    Hello World!/ # 
    ```
    Then we can exit, and check current pods.
    ```
    $ microk8s.kubectl get pods
    NAME     READY   STATUS      RESTARTS   AGE
    alpine   0/1     Completed   0          5m23s
    my-pod   1/1     Running     0          77m
    ```

---
Here is a list of some useful commands for kubectl.

1. get pods
2. describe
3. expose 
4. port-forward
5. attach
6. exec
7. label
