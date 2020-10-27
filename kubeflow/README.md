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
