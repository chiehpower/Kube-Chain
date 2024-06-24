# Use GKE

1. Install Kubectl (not recorded in this section.)
2. Setup the config
    ```
    gcloud config set project [PROJECT_ID]
    gcloud config set compute/zone <region>
    ```
3. Create a cluster.
    ```
    gcloud container clusters create loadbalancedcluster --num-nodes=2
    ```
    GKE will create 3 nodes by a default setting. We change to use 2 nodes.
    ```
    Default change: VPC-native is the default mode during cluster creation for versions greater than 1.21.0-gke.1500. To create advanced routes based clusters, please pass the `--no-enable-ip-alias` flag
    Note: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s).
    Creating cluster loadbalancedcluster in asia-east1-a... Cluster is being health-checked (master is healthy)...done.                                      
    To inspect the contents of your cluster, 
    kubeconfig entry generated for loadbalancedcluster.
    NAME                 LOCATION      MASTER_VERSION      MASTER_IP       MACHINE_TYPE  NODE_VERSION        NUM_NODES  STATUS
    loadbalancedcluster  asia-east1-a  1.29.4-gke.1043002  xxxxxxxxxxxxxx  e2-medium     1.29.4-gke.1043002  2          RUNNING
    ```

4. Check all nodes from this cluster.
    ```
    gcloud compute instances list | grep gke  
    ```

    Output:
    ```
    gke-loadbalancedcluster-default-pool-8m2k  asia-east1-a  e2-medium                   10.140.2.46  xxxxx   RUNNING
    gke-loadbalancedcluster-default-pool-x2nb  asia-east1-a  e2-medium                   10.140.2.47  xxxxx   RUNNING
    ```