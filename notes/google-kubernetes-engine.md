# Google Kubernetes engine

* **Managed** Kubernetes service
* Minimizes operations with **auto-repair** (repair failed nodes)
* **Auto-upgrade** features (use latest version of K8S always)
* **Auto-scaling** for **Pods** and **Cluster**  
* Enable Cloud logging & cloud monitoring with simple configurations
* Use **container-optimized OS** - hardened OS built by Google
* Provids support for persistent disks and local SSDs

## Masters & Workers
* **Cluster**: Group of compute engine instances
* **Master Nodes**: Manages the cluster
  * **API Server**: Handles all communication for K8S cluster (from nodes & outside)
  * **Schedular**: Decides placement of pods
  * **Control Manager**: Manages deployments & replicasets
  * **etcd**: Distributed database storing the cluster state
* **Worker Nodes**: Runs your workloads (Pods)
  * **Kubelet**: Manages communications with master nodes


## Type of GKE Clusters
* **Zonal Cluster**:
  * Single Zonal - Single control plane. Nodes running in same zone
  * Multi Zonal - Single control plane but nodes running in multiple zones
* **Regional Cluster**: Replicas of control plances runs in multiple zones of a given region. Nodes also runs in same zones where control plane runs
* **Private Cluster**: VPC-native cluster. Nodes only have internal IP addresses
* **Alpha Cluster**: Clusters with Alpha APIs - early feature API's. Used to test new K8S features.


## Kubernetes Pods
* **Smallest deployable unit** in kubernetes
* Pod **contains one or more containers**  
* Each pod is assigned **ephemeral IP**  
* All containers in pod shares ----> **network, storage, IP address, ports, volumes**  
* Pod statuses ----> **running/pending/succeeded/failed/unknown**  
  
![image](https://github.com/ramkrushna26/gcp/assets/45620457/6eab8d89-5bc1-4260-b653-0fa166f7a2a2)

  
## Kubernetes Deployments
```
# Updating deployments with new image
$ kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE
```
```
$ kubctl create deployment m1 --image=m1:v1
```
* Deployment is created for each micro-service with all its releases
  * deployment manages new releases with **zero downtime**
```
$ kubctl scale deployment m2 --replicas=2
```
* **Replicasets** ensures that specific number of pods are running for specific micro-service version
  * even if one the pods is killed, replicasets will launch a new one
```
$ kubctl set image deployment m1 m1=m1:v2
```
* Deploy V2 of micro-service - creates a new replica set
  * V2 replica set is created
  * deployment udpates v1 replicaset and v2 replica set based on the release strategies

## Kubernetes - Services
* Each pods has its **own IP** address
  * How do you ensure that external users are not impacted when:
    * a pod fails and replaced by replica set
    * a new release happens and all the existing pods of old releases are replaced by one of new release
  
```
$ kubctl expose deployment name --type=LoadBalancer --port=80
```
* Create **Service**
  * Expose pods to outside world using stable IP address
  * Ensures that external world does NOT get impacted as pods go down and come up
* Three types:
  * **ClusterIP**: expose service on a cluster internal IP
    * use case: you want your micro-service only to be available inside the cluster (intra cluster communication)
  * **LoadBalancer**: expose service externally using cloud providers load balancer
    * use case: you want to create individual load balancer for each microservice
  * **NodePort**: Expose service on each node IP as a static port (the NodePort)
    * use case: you do not want to create an external load balancer for each micro-service (you can create one ingress component to load balance multiple micro-service

  
## Container Registry - Image Repository
![image](https://github.com/ramkrushna26/gcp/assets/45620457/d727b2ae-c76a-4c9a-9118-b4692bff827a)

* **Container Registry**: fully managed container registry provided by GCP
  * alternative --> DocketHub
  * storage for your docker images
  * can be integrated to CI/CD tools to publish images to registry
  * can secure container images. Analyse for vulnerabilities and enforce deployment policies
  * Naming: Hostname/ProjectId/Image:Tag-gcr.io/projectname/helloworld:1 

## GKE - Points to remember
* **Replicate master nodes** across multiple zones for high availibility
* Some CPU on the nodes is **reserved by control plane**
  * 1st core-6%, 2nd core-1%, 3/4 core-0.5%, rest-0.25%
* creating docker image for your micro-services (dockerfile)
```
# built image
$ docker build -t domain/hello-rest-api:0.0.1.RELEASE
#test it locally
$ docker run -d -p 8080:8080 domain/hello-rest-api:0.0.1.RELEASE
#push it to container repo
$ docker push -d -p 8080:8080 domain/hello-rest-api:0.0.1.RELEASE
```
* Kubernetes support **stateful** deployment like kafka, redis, zookeeper
  * **StatefulSet**: set of pods with unique and consistent identities and stable hostnames
* How do we run services on nodes for log collection or monitoring?
  * **DaemonSet**: one pod on every node (for background service)
* Enabled by default --> integrated with cloud monitoring & cloud logging
  * cloud logging system & application logs can be exported to BigQUery & Pub/Sub

 
## GKE - Scenarios
* You want to keep your cost low & optimize GKE implementation
> Consider preemptible (short-lived) VMs, appropriate region, commited use discounts  
> E2 machine types are cheaper than N1  
> Choose right environment to fit your workload type (use multiple node pools if needed)  

* You want efficient, completely auto scaling GKE solution
> configure horizontal pod autoscaler for deployments & cluster auto scalar for node pools

* You want to execute untrusted third-party code in kubernetes cluster
> create a node pool with GKE sandbox. deploy untrusted node pool to sandbox node pool  

* You want to enable internal only communication between your micro-service deployments in kubernetes cluster
> create service of type ClusterIP

* My pod stays pending
> Most probably pods can not be scheduled onto a node (insufficient resources)

* Pods stays waiting
> Mostly due to failure to pull an image

## GKE - Cluster management commands
* create user
```
$ gcloud container clusters create <cluster> --zone us-central1-a --node-locations us-central1-b
```
* Resize cluster 
```
$ gcloud container clusters resize <cluster> --node-pool <node pool> --num-nodes 10
```
* Autoscale cluster
```
$ gcloud container clusters update <cluster> --enable-autoscaling --min-nodes=1 --max-nodes=10
```
* Delete cluster
```
$ gcloud container clusters delete <cluster>
```
* Adding node pools
```
$ gcloud container node-pools <node pool> --cluster <cluster>
```
* List Images
```
$ gcloud container images list
```

## GKE - Workload management commands
* List pods/replicasets/service
```
$ kubctl get pods/services/replicasets
```
* create deployments
```
$ kubctl apply -f deployment.yaml
OR
$ kubctl create deployment 
```
* create service
```
$ kubctl expose deployment hello-rest-api --type=LoadBalancer --port=8080
```
* scale deployment
```
$ kubctl scale deployment hello-api --replicas 5
```
* autoscale deployment
```
$ kubctl autoscale deployment --max --min --cpu-percent
```
* delete deployment
```
$ kubctl delete deployment hello-api
```
* update deployment 
```
$ kubctl apply -f deployment.yaml
```
* Rollback deployment
```
$ kubctl rollour undo deployment hello-api --to-revision=1
```

## Creating Kubernetes Clusters
### Autopilot mode -- Nodes, Networking, Security & Telemetry taken care by Google
```
$ gcloud beta container --project "my-kubernetes-project-412915" clusters create-auto "autopilot-cluster-1" --region "us-central1" --release-channel "regular" --network "projects/my-kubernetes-project-412915/global/networks/default" --subnetwork "projects/my-kubernetes-project-412915/regions/us-central1/subnetworks/default" --cluster-ipv4-cidr "/17" --binauthz-evaluation-mode=DISABLED
```
### Creating kubernetes cluster
```
$ glcoud container clusters create
```
### Connecting to kubernetes cluster
```
$ gcloud container clusters get-credentials <cluster name> --region us-central1 --project <project name>
```
### Deploying microservice to kubernetes cluster
```
$ kubectl create deployment hello-world-rest-api --image=domain/hello-rest-api:0.0.1.RELEASE

# to check deployments
$ kubectl get deployment 
```
### Exposing kubernetes cluster to load balancer
```
$ kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080

# to get services info
$ kubectl get services --watch  

# to test services
$ curl external ip:8080 
```
### Scaling instances of microservice
```
$ kubectl scale deployment hello-world-rest-api --replicas=2

# To get all pods
$ kubectl get pods

# to watch the web traffic
$ watch curl http://35.202.88.22:8080/hello-worl
```
### Increasing nodes in kubernetes cluster
```
$ gcloud container clusters resize <cluster name> --node-pool default-pool --num-nodes=5 --zone=us-central1-c
```
### Auto-scaling micro-services
```
$ kubectl autoscale deployment hello-world-rest-api --max=10 --cpu-percent=70

$ kubectl get hpa
```
### Auto-scaling kubernetes cluster
```
$ gcloud container clusters udpate hello-world-rest-api --enable-autoscaling --min-nodes=1 --max-nodes=10
```
### Adding application configuration for micro-services
```
$ kubectl create configmap todo-web-application-config --from-literal=RDS_DB_NAME=todos
```
### Configuring passwords for micro-services
```
$ kubectl create secrets generic todo-web-application-config-secrets-1 --from-literal=RDS_PASSWORDS=dummytodos

$ kubectl get secret
```
### Deploying through yaml
```
$ kubectl apply -f deployment.yaml
```
## Sample deployment yaml
![image](https://github.com/ramkrushna26/gcp/assets/45620457/993cdfaa-c263-475e-b963-3653488d8f8e)

## Sample service yaml
![image](https://github.com/ramkrushna26/gcp/assets/45620457/5ebef679-e5cc-4997-afd0-e5fa60a52441)

## Deploying micro-services with nodes GPU attached
```
$ gcloud container node-pools create <pool name> --cluster <cluster name>
$ gcloud container note-pools list --cluster <cluster name> --region <region>

# Deploy new micro-service to new pool by setting nodeSelector in deployment.yaml
nodeSelector: cloud.google.com/gke-nodepool:<pool name>
```
## Deleting micro-services
```
$ kubectl delete service <service name>
$ kubectl delete deployment <name>
```
## Deleting GKE clusters/services/deployments
```
$ kubectl delete service <service name>
$ kubectl delete deployment <deployment name>
$ gcloud container clusters delete <cluster name> --zone/--region <zone/region name>
```
