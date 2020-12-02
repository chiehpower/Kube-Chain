# Install Kubeflow

All source from : [Official tutorial](https://www.kubeflow.org/docs/pipelines/installation/localcluster-deployment/)

```
$ kind get clusters
kind

$ kind version
kind v0.9.0 go1.15.2 linux/amd64
```

Creating a cluster on kind
```
kind create cluster
```

This will bootstrap a Kubernetes cluster using a pre-built node image.
You can find that image on the Docker Hub kindest/node here.
If you wish to build the node image yourself, you can use the kind build node-image command—see the official building image section for more details. And, to specify another image, use the --image flag.

By default, the cluster will be given the name kind. Use the --name flag to assign the cluster a different context name.

### Deploying Kubeflow Pipelines

To deploy the Kubeflow Pipelines, run the following commands:
```
export PIPELINE_VERSION=1.0.4
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION"
```
Output:
```
namespace/kubeflow created
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/applications.app.k8s.io created
customresourcedefinition.apiextensions.k8s.io/clusterworkflowtemplates.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/cronworkflows.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/scheduledworkflows.kubeflow.org created
customresourcedefinition.apiextensions.k8s.io/viewers.kubeflow.org created
customresourcedefinition.apiextensions.k8s.io/workflows.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/workflowtemplates.argoproj.io created
serviceaccount/kubeflow-pipelines-cache-deployer-sa created
clusterrole.rbac.authorization.k8s.io/kubeflow-pipelines-cache-deployer-clusterrole created
clusterrolebinding.rbac.authorization.k8s.io/kubeflow-pipelines-cache-deployer-clusterrolebinding created
```
Commands:
```
kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic-pns?ref=$PIPELINE_VERSION"
```
Output:
```
serviceaccount/application created
serviceaccount/argo created
serviceaccount/kubeflow-pipelines-cache created
serviceaccount/kubeflow-pipelines-container-builder created
serviceaccount/kubeflow-pipelines-metadata-writer created
serviceaccount/kubeflow-pipelines-viewer created
serviceaccount/ml-pipeline-persistenceagent created
serviceaccount/ml-pipeline-scheduledworkflow created
serviceaccount/ml-pipeline-ui created
serviceaccount/ml-pipeline-viewer-crd-service-account created
serviceaccount/ml-pipeline-visualizationserver created
serviceaccount/ml-pipeline created
serviceaccount/pipeline-runner created
role.rbac.authorization.k8s.io/application-manager-role created
role.rbac.authorization.k8s.io/argo-role created
role.rbac.authorization.k8s.io/kubeflow-pipelines-cache-deployer-role created
role.rbac.authorization.k8s.io/kubeflow-pipelines-cache-role created
role.rbac.authorization.k8s.io/kubeflow-pipelines-metadata-writer-role created
role.rbac.authorization.k8s.io/ml-pipeline-persistenceagent-role created
role.rbac.authorization.k8s.io/ml-pipeline-scheduledworkflow-role created
role.rbac.authorization.k8s.io/ml-pipeline-ui created
role.rbac.authorization.k8s.io/ml-pipeline-viewer-controller-role created
role.rbac.authorization.k8s.io/ml-pipeline created
role.rbac.authorization.k8s.io/pipeline-runner created
rolebinding.rbac.authorization.k8s.io/application-manager-rolebinding created
rolebinding.rbac.authorization.k8s.io/argo-binding created
rolebinding.rbac.authorization.k8s.io/kubeflow-pipelines-cache-binding created
rolebinding.rbac.authorization.k8s.io/kubeflow-pipelines-cache-deployer-rolebinding created
rolebinding.rbac.authorization.k8s.io/kubeflow-pipelines-metadata-writer-binding created
rolebinding.rbac.authorization.k8s.io/ml-pipeline-persistenceagent-binding created
rolebinding.rbac.authorization.k8s.io/ml-pipeline-scheduledworkflow-binding created
rolebinding.rbac.authorization.k8s.io/ml-pipeline-ui created
rolebinding.rbac.authorization.k8s.io/ml-pipeline-viewer-crd-binding created
rolebinding.rbac.authorization.k8s.io/ml-pipeline created
rolebinding.rbac.authorization.k8s.io/pipeline-runner-binding created
configmap/metadata-grpc-configmap created
configmap/ml-pipeline-ui-configmap created
configmap/pipeline-install-config-7t84h8df7d created
configmap/workflow-controller-configmap created
secret/mlpipeline-minio-artifact created
secret/mysql-secret-fd5gktm75t created
service/cache-server created
service/controller-manager-service created
service/metadata-envoy-service created
service/metadata-grpc-service created
service/minio-service created
service/ml-pipeline-ui created
service/ml-pipeline-visualizationserver created
service/ml-pipeline created
service/mysql created
deployment.apps/cache-deployer-deployment created
deployment.apps/cache-server created
deployment.apps/controller-manager created
deployment.apps/metadata-envoy-deployment created
deployment.apps/metadata-grpc-deployment created
deployment.apps/metadata-writer created
deployment.apps/minio created
deployment.apps/ml-pipeline-persistenceagent created
deployment.apps/ml-pipeline-scheduledworkflow created
deployment.apps/ml-pipeline-ui created
deployment.apps/ml-pipeline-viewer-crd created
deployment.apps/ml-pipeline-visualizationserver created
deployment.apps/ml-pipeline created
deployment.apps/mysql created
deployment.apps/workflow-controller created
application.app.k8s.io/pipeline created
persistentvolumeclaim/minio-pvc created
persistentvolumeclaim/mysql-pv-claim created
```

The Kubeflow Pipelines deployment may take several minutes to complete.

```
kubectl port-forward -n kubeflow svc/ml-pipeline-ui 8080:80
```

Output: 
```
Forwarding from 127.0.0.1:8080 -> 3000
Forwarding from [::1]:8080 -> 3000
E1027 16:19:07.906784  796197 portforward.go:233] lost connection to pod
```

---
# Update - try on GCP

Followed the instructions from [Set up a Google Cloud Project](https://www.kubeflow.org/docs/gke/deploy/project-setup/).

1. Create a project

2. Go to enable these APIs below from here.
![png](assets/api.png)  

* [Compute Engine API](https://console.cloud.google.com/apis/library/compute.googleapis.com)
* [Kubernetes Engine API](https://console.cloud.google.com/apis/library/container.googleapis.com)
* [Identity and Access Management (IAM) API](https://console.cloud.google.com/apis/library/iam.googleapis.com)
* [Service Management API](https://console.cloud.google.com/apis/api/servicemanagement.googleapis.com)
* [Cloud Resource Manager API](https://console.developers.google.com/apis/library/cloudresourcemanager.googleapis.com)
* [AI Platform Training & Prediction API](https://console.developers.google.com/apis/library/ml.googleapis.com)
* [Cloud Build API](https://console.cloud.google.com/apis/library/cloudbuild.googleapis.com) (It’s required if you plan to use [Fairing](https://www.kubeflow.org/docs/components/fairing/) in your Kubeflow cluster)

Of course, you can use commands on cloud shell.
```
gcloud services enable \
  compute.googleapis.com \
  container.googleapis.com \
  iam.googleapis.com \
  servicemanagement.googleapis.com \
  cloudresourcemanager.googleapis.com \
  ml.googleapis.com

# Cloud Build API is optional, you need it if using Fairing.
# gcloud services enable cloudbuild.googleapis.com
```

3. Please do anything via cloud shell and you can follow here. https://www.kubeflow.org/docs/started/k8s/kfctl-k8s-istio/ for prepare your environment.
But the differences are listed below:

- Use this version `v1.0-rc.1` : https://github.com/kubeflow/kfctl/releases/tag/v1.0-rc.1
- export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v0.7-branch/kfdef/kfctl_k8s_istio.0.7.1.yaml"
- the cluster of kubectl version : `v1.15.12` (Actually dont use over v1.15 version) (i.e., v1.16 cannot work)

---
## Error 

Run on cloud shell with kubectl **v1.16**
```
***************************************************************
Notice anonymous usage reporting enabled using spartakus
To disable it
If you have already deployed it run the following commands:
  cd $(pwd)
  kubectl -n ${K8S_NAMESPACE} delete deploy -l app=spartakus

For more info: https://www.kubeflow.org/docs/other-guides/usage-reporting/
****************************************************************
```

```
WARN[0305] Encountered error during apply:  (kubeflow.error): Code 500 with message: Apply.Run  Error error when creating "/tmp/kout162494271": CustomResourceDefinition.apiextensions.k8s.io "seldondeployments.machinelearning.seldon.io" is invalid: [spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[explainer].properties[endpoint].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].properties[children].items.properties[children].items.properties[children].items.type: Required value: must not be empty for specified array items, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].properties[children].items.properties[children].items.properties[endpoint].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].properties[children].items.properties[children].items.type: Required value: must not be empty for specified array items, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].properties[children].items.properties[endpoint].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].properties[children].items.type: Required value: must not be empty for specified array items, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].properties[endpoint].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[graph].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[env].items.properties[valueFrom].properties[configMapKeyRef].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[env].items.properties[valueFrom].properties[fieldRef].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[env].items.properties[valueFrom].properties[resourceFieldRef].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[env].items.properties[valueFrom].properties[secretKeyRef].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[env].items.properties[valueFrom].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[env].items.type: Required value: must not be empty for specified array items, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.properties[svcOrchSpec].properties[resources].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.properties[spec].properties[predictors].items.type: Required value: must not be empty for specified array items, spec.validation.openAPIV3Schema.properties[spec].type: Required value: must not be empty for specified object fields, spec.validation.openAPIV3Schema.type: Required value: must not be empty at the root]  filename="kustomize/kustomize.go:193"
```

The reason is because of the version of cluster which was used v1.16.

---
## Downgrade the version of kubectl to v1.15.12

After I downgraded to `v1.15.12`, it could succeeded.
```
chiehtsaiphysicist@cloudshell:~/kf-test-11 (kubeflow-297403)$ kubectl version
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.4", GitCommit:"d360454c9bcd1634cf4cc52d1867af5491dc9c5f", GitTreeState:"clean", BuildDate:"2020-11-11T13:17:17Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"15+", GitVersion:"v1.15.12-gke.20", GitCommit:"0ac5f81eecab42bff5ef74f18b99d8896ba7b89b", GitTreeState:"clean", BuildDate:"2020-09-09T00:48:20Z", GoVersion:"go1.12.17b4", Compiler:"gc", Platform:"linux/amd64"}
```

Finally ...
```
serviceaccount/seldon-manager created
clusterrole.rbac.authorization.k8s.io/seldon-operator-manager-role created
clusterrolebinding.rbac.authorization.k8s.io/seldon-operator-manager-rolebinding created
configmap/seldon-config created
secret/seldon-operator-webhook-server-secret created
service/seldon-operator-controller-manager-service created
service/webhook-server-service created
statefulset.apps/seldon-operator-controller-manager created
application.app.k8s.io/seldon-core-operator created
INFO[0257] Applied the configuration Successfully!       filename="cmd/apply.go:72"
```

Check pod whether it can deploy successfully.
```
kubectl get po -n kubeflow
```
Output:
```
NAME                                                           READY   STATUS    RESTARTS   AGE
admission-webhook-bootstrap-stateful-set-0                     1/1     Running   0          6m31s
admission-webhook-deployment-b7d89f4c7-vjvs5                   1/1     Running   0          5m32s
application-controller-stateful-set-0                          1/1     Running   0          6m51s
argo-ui-6754c76f9b-8g766                                       1/1     Running   0          6m42s
centraldashboard-5578cc9569-c97c2                              1/1     Running   0          6m34s
jupyter-web-app-deployment-6b7d9c5fd6-wz9dj                    1/1     Running   0          6m21s
katib-controller-789d76d446-vgcnb                              1/1     Running   1          5m3s
katib-db-75975d8dbd-jkvfr                                      1/1     Running   0          5m2s
katib-manager-59bb84948f-r988j                                 1/1     Running   0          5m1s
katib-ui-dd75bd446-g8q67                                       1/1     Running   0          5m1s
kfserving-controller-manager-0                                 2/2     Running   1          5m26s
metacontroller-0                                               1/1     Running   0          6m47s
metadata-db-7584d44b65-5sfgn                                   1/1     Running   0          6m14s
metadata-deployment-cd8f7d58f-6dcp6                            1/1     Running   0          6m13s
metadata-envoy-deployment-bff4f8b9-ctfwc                       1/1     Running   0          6m13s
metadata-grpc-deployment-7cc5d84854-g59g2                      1/1     Running   4          6m12s
metadata-ui-7c978889b5-67kvb                                   1/1     Running   0          6m12s
minio-764648495-rgtfz                                          1/1     Running   0          4m53s
ml-pipeline-588b64fff-8lpdp                                    1/1     Running   0          4m56s
ml-pipeline-ml-pipeline-visualizationserver-6c7c97869d-k8mhk   1/1     Running   0          4m31s
ml-pipeline-persistenceagent-79ff896578-v6lw4                  1/1     Running   0          4m48s
ml-pipeline-scheduledworkflow-7d89bb6db5-7hw6d                 1/1     Running   0          4m32s
ml-pipeline-ui-6656886579-gg944                                1/1     Running   0          4m42s
ml-pipeline-viewer-controller-deployment-546bd5f545-gl7v2      1/1     Running   0          4m37s
mysql-6c9cb88c4d-ckchq                                         1/1     Running   0          4m50s
notebook-controller-deployment-6d594ddd6b-7jw9c                1/1     Running   0          6m5s
profiles-deployment-67799585bd-5wxwp                           2/2     Running   0          4m27s
pytorch-operator-fdfd7985-f6w8q                                1/1     Running   0          6m1s
seldon-operator-controller-manager-0                           1/1     Running   1          4m19s
spartakus-volunteer-5888bc655-kwfsd                            1/1     Running   0          5m23s
tensorboard-5f685f9d79-5nrzd                                   1/1     Running   0          5m21s
tf-job-operator-5dff84b966-rzmtl                               1/1     Running   0          5m15s
workflow-controller-85c665bcb9-vnm2h                           1/1     Running   0          6m41s
```

Check whether istio status is.
```
chiehtsaiphysicist@cloudshell:~/kf-test-11 (kubeflow-297403)$ kubectl get po -n istio-system
NAME                                      READY   STATUS      RESTARTS   AGE
grafana-86f89dbd84-89p5t                  1/1     Running     0          8m4s
istio-citadel-74966f47d6-r927d            1/1     Running     0          8m4s
istio-cleanup-secrets-1.1.6-5jvdl         0/1     Completed   0          7m53s
istio-egressgateway-5c64d575bc-khjhm      1/1     Running     0          8m3s
istio-egressgateway-5c64d575bc-nqqcs      1/1     Running     0          5m4s
istio-galley-784b9f6d75-fxgw2             1/1     Running     0          8m3s
istio-grafana-post-install-1.1.6-fssv7    0/1     Completed   0          7m52s
istio-ingressgateway-589ff776dd-b828z     1/1     Running     0          8m2s
istio-ingressgateway-589ff776dd-x2vvz     1/1     Running     0          4m33s
istio-pilot-677df6b6d4-4h9sl              2/2     Running     0          8m2s
istio-pilot-677df6b6d4-dp6tp              2/2     Running     0          4m49s
istio-pilot-677df6b6d4-mpx7x              2/2     Running     0          5m3s
istio-pilot-677df6b6d4-nbmt2              2/2     Running     0          5m2s
istio-pilot-677df6b6d4-rr4jh              2/2     Running     0          5m2s
istio-policy-6f74d9d95d-cr6f6             2/2     Running     2          8m1s
istio-security-post-install-1.1.6-vxvrd   0/1     Completed   0          7m52s
istio-sidecar-injector-866f4b98c7-65sw6   1/1     Running     0          8m1s
istio-telemetry-549c8f9dcb-6wccl          2/2     Running     2          8m
istio-tracing-555cf644d-nx4pz             1/1     Running     0          8m
kiali-7db44d6dfb-v88s6                    1/1     Running     0          7m59s
prometheus-d44645598-v7ghg                1/1     Running     0          7m59s
```

---
## Create Service Account

```
gcloud iam service-accounts create gcp-sa # $GSA_NAME
kubectl create serviceaccount --namespace kubeflow k8s-sa # $KSA_NAME

gcloud iam service-accounts add-iam-policy-binding   --role roles/iam.workloadIdentityUser   --member "serviceAccount:kubeflow-297403.svc.id.goog[kubeflow/k8s-sa]"   gcp-sa@kubeflow-297403.iam.gserviceaccount.com

kubectl annotate serviceaccount \
  --namespace kubeflow \
  k8s-sa \
  iam.gke.io/gcp-service-account=gcp-sa]@kubeflow-297403.iam.gserviceaccount.com
```

Until this part, the Kubeflow was deployed done!

---
## Start KubeFlow

```
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
```
![kf](assets/kf.png)
![kf1](assets/kf2.png) 
![kf2](assets/kf3.png) 
![kf3](assets/kf4.png)

We can test this example of mnist on gcp.
Download from here: `git clone https://github.com/kubeflow/examples.git git_kubeflow-examples`

![jp](assets/jp.png)

---
## Run mnist example

#### Error 1
When we run on this line, we can use `pip` to install it.

```
ImportError: You need to install 'msrestazure' to use this feature
```

Ref: https://github.com/kubeflow/examples/issues/828

#### Error 2
```
AttributeError: module 'tornado.ioloop' has no attribute '_Selectable'
```
Try to use this version `tornado>=6.0.3`