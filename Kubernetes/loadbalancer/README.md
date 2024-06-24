# Steps 

1. Start a cluster
    ```
    gcloud container clusters create loadbalancedcluster --num-nodes=2
    ```
2. Get the authorization
    ```
    gcloud container clusters get-credentials loadbalancedcluster
    ```
    Output
    ```
    Fetching cluster endpoint and auth data.
    kubeconfig entry generated for loadbalancedcluster.
    ```
3. Build a docker image
    ```
    docker build -t gcr.io/${PROJECT_ID}/hello-app:foo . 
    docker push gcr.io/${PROJECT_ID}/hello-app:foo
    ```
    Please change the return results to `bar` in the `main.go`. 
    give a new tag and push to GCP as well.
4. Run the services
    ```
    $ kubectl apply -f pod.yaml,service.yaml
    pod/foo created
    pod/bar created
    service/my-loadbalancer created
    ```
5. Check the services
    ```
    $ kubectl get services
    NAME              TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
    kubernetes        ClusterIP      34.118.224.1    <none>          443/TCP          80m
    my-loadbalancer   LoadBalancer   34.118.237.12   34.81.134.118   8080:32736/TCP   113s
    ```