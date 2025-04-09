Creating this repo with an intent to make Kubernetes easy for begineers. This is a work-in-progress repo.

# Kubernetes Architecture:-
![1687453579458](https://github.com/user-attachments/assets/a56cc757-1047-411f-a844-931cf5afabf1)


A Kubernetes cluster consists of two types of nodes: master and worker nodes. 

The master node hosts the Kubernetes control plane and manages the cluster, including scheduling and scaling applications and maintaining the state of the cluster. 

The worker nodes are responsible for running the containers and executing the workloads.

The master node has several components, such as:

API server: This is the main component that exposes the Kubernetes API and communicates with other components. It is the endpoint that the Kubernetes CLI (kubectl) and other clients talk to when creating or managing resources.

etcd: This is a distributed key-value store that stores the cluster state and configuration data. It is the source of truth for the cluster.

Controller manager: This runs multiple controller processes that watch for changes in the desired state of the cluster and take actions to make it happen. For example, it can create or delete pods, services, or endpoints.

Scheduler: This assigns pods to worker nodes based on various criteria, such as resource requirements, labels, or affinity rules. It works with the API server to schedule the workloads on the cluster.

Cloud controller manager: This runs controllers that are specific to the cloud provider and can manage resources outside of the cluster, such as nodes, load balancers, or routes. This component only runs if the cluster is running in the cloud.

The worker node has these components:

Kubelet: This is an agent that runs on each worker node and communicates with the API server. It manages the containers and pods on the node, ensuring that they are running and healthy. It also reports the node status and resources to the master node.

Container runtime: This is responsible for working with the containers and executing them. It can be Docker or another container runtime, such as containerd or cri-o. It uses the container runtime interface (CRI) to communicate with the kubelet.

Pods: These are groups of one or more containers that share storage and network resources, and a specification for how to run them. Pods are the smallest units of a Kubernetes application. They can be created and managed by workload resources, such as deployments or statefulsets.

Kube-proxy: This is a network proxy that runs on each worker node and enforces network rules on them. It helps Kubernetes in managing the connectivity among pods and services. It also acts as an egress-based load-balancing controller that monitors the Kubernetes API server and updates node’s iptables subsystem based on it.

Kubernetes Installation Using KOPS on EC2

Create an EC2 instance or use your personal laptop.

Dependencies required :-
1 Python3
2 AWS CLI
3 kubectl


Install dependencies :- 

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y python3-pip apt-transport-https kubectl

pip3 install awscli --upgrade

export PATH="$PATH:/home/ubuntu/.local/bin/"

Install KOPS :- 

curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops-linux-amd64

sudo mv kops-linux-amd64 /usr/local/bin/kops

Provide the below permissions to your IAM user. If you are using the admin user, the below permissions are available by default
AmazonEC2FullAccess
AmazonS3FullAccess
IAMFullAccess
AmazonVPCFullAccess

Set up AWS CLI configuration on your EC2 Instance or Laptop.
Run aws configure

Kubernetes Cluster Installation

Please follow the steps carefully and read each command before executing.

Create S3 bucket for storing the KOPS objects.

aws s3api create-bucket --bucket kops-gur-storage --region us-east-1

Create the cluster:-
kops create cluster --name=demok8scluster.k8s.local --state=s3://kops-gur-storage --zones=us-east-1a --node-count=1 --node-size=t2.micro --master-size=t2.micro  --master-volume-size=8 --node-volume-size=8

Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.

kops edit cluster myfirstcluster.k8s.local

Step 12: Build the cluster

kops update cluster demok8scluster.k8s.local --yes --state=s3://kops-gur-storage
This will take a few minutes to create............

After a few mins, run the below command to verify the cluster installation.

kops validate cluster demok8scluster.k8s.local
About
Creating this repo with an intent to make Kubernetes easy for begineers. This is a work-in-progress repo.



How to Install Minikube using Chocolatey on Windows

Step 1: Install Minikube

choco install minikube

Step 2: Start Your Cluster

Make sure Docker is installed on your system before starting Minikube.

minikube start --driver=docker

Step 3: Install kubectl Command Using curl

Install curl Using Chocolatey

choco install curl

Download kubectl Binary

If you already have curl installed, use the following command:

curl.exe -LO "https://dl.k8s.io/release/v1.32.0/bin/windows/amd64/kubectl.exe"

Step 4: Validate the Binary

curl.exe -LO "https://dl.k8s.io/v1.32.0/bin/windows/amd64/kubectl.exe.sha256"

Step 5: Add kubectl to PATH Environment Variable

Append or prepend the kubectl binary folder to your PATH environment variable.

Step 6: Verify kubectl Version

Ensure the installed kubectl version matches the one you downloaded:

kubectl version --client

Accessing Your Minikube Cluster

To access your Minikube cluster and interact with your application:

SSH into Minikube:

minikube ssh

Use curl to access your application:

curl <Pod_IP>

# Managing Kubernetes Resources

# Creating a Pod

- Use a YAML file to create a pod:

kubectl create -f pod.yml

# Checking Pod Status

- To check the status of all pods:

kubectl get pods

- To get more details about the pods:

kubectl get pods -o wide

# Deleting a Pod

- To delete a specific pod:

kubectl delete pod <pod-name>

- List All Resources in a Particular Namespace

kubectl get all

- List All Resources from All Namespaces

kubectl get all -A

- List Deployments

kubectl get deployment

- List ReplicaSets

kubectl get rs

Watch Pods in Real-Time

kubectl get pods -w

Applying Changes to Deployment

If you make changes to your deployment file, apply them using:

kubectl apply -f <filename>.yml

Notes:- 

To find all kubectl commands, search for kubectl cheatsheet on Google.



# Service in Kubernetes
Kubernetes Service is used to expose an application deployed on a set of pods using a single endpoint. Services are introduced to provide reliable networking by bringing stable IP addresses and DNS names to ephemeral pods. Service enables network access to a set of Pods in Kubernetes.

Why Services in Kubernetes?
In Kubernetes, each Pod gets its own internal IP address, but Pods are ephemeral (not constant). Pods are frequently created and destroyed, causing their IP addresses to change constantly.

Non-functioning pods get replaced by new ones automatically. Meaning that when old Pod dies and new one gets started in its place it gets a new IP address. So it doesn’t make sense to use Pod IP addresses directly, because then you would have to adjust that every time the Pod gets recreated. It will create discoverability issues for the deployed application in pods, and making it difficult to identify which pods to connect.

With the Service component you have a solution of a stable or static IP address that stays even when the Pod is destroyed. Basically we set a Service in front of each Pod, which represents a stable IP address. So clients can call a single stable IP address instead…


# There are four types of Kubernetes services — 
   1 ClusterIP 
   2 NodePort 
   3 LoadBalancer 
   4 ExternalName. 
   
Note :- The type property in the Service's spec determines how the service is exposed to the network.

1. ClusterIP
ClusterIP is the default and most common service type.
Kubernetes will assign a cluster-internal IP address to ClusterIP service. This makes the service only reachable within the cluster.
You cannot make requests to service (pods) from outside the cluster.
You can optionally set cluster IP in the service definition file.

Use Cases :- 
Inter service communication within the cluster. For example, communication between the front-end and back-end components of your app. Eg:- day3.yml

2. NodePort
NodePort service is an extension of ClusterIP service. A ClusterIP Service, to which the NodePort Service routes, is automatically created.
It exposes the service outside of the cluster by adding a cluster-wide port on top of ClusterIP.
NodePort exposes the service on each Node’s IP at a static port (the NodePort). Each node proxies that port into your Service. So, external traffic has access to fixed port on each Node. It means any request to your cluster on that port gets forwarded to the service.
You can contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>.
Node port must be in the range of 30000–32767. Manually allocating a port to the service is optional. If it is undefined, Kubernetes will automatically assign one.
If you are going to choose node port explicitly, ensure that the port was not already used by another service.

Use Cases:-
When you want to enable external connectivity to your service.
Using a NodePort gives you the freedom to set up your own load balancing solution, to configure environments that are not fully supported by Kubernetes, or even to expose one or more nodes’ IPs directly.
Prefer to place a load balancer above your nodes to avoid node failure.
Eg:- day4.yml

3. LoadBalancer
LoadBalancer service is an extension of NodePort service. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.
It integrates NodePort with cloud-based load balancers.
It exposes the Service externally using a cloud provider’s load balancer.
Each cloud provider (AWS, Azure, GCP, etc) has its own native load balancer implementation. The cloud provider will create a load balancer, which then automatically routes requests to your Kubernetes Service.
Traffic from the external load balancer is directed at the backend Pods. The cloud provider decides how it is load balanced.
The actual creation of the load balancer happens asynchronously.
Every time you want to expose a service to the outside world, you have to create a new LoadBalancer and get an IP address.

Use Cases:- 
When you are using a cloud provider to host your Kubernetes cluster.
This type of service is typically heavily dependent on the cloud provider.
Eg:- day5.yml

- How to list services
  kubectl get svc

- How to see the information about services
  kubectl get svc -v=7 

- To check Minikube IP
  minikube ip

#  If you are running minikube on window just use this command to deploy your application 
   minikube service flask-app-service # This command creates a temporary tunnel to expose your service
   
[def]: <kubernetes architecture.jfif>
[def2]: <kubernetes architecture-1.jfif>


# What is an Ingress?
Ingress is a Kubernetes resource type, that can be applied just like other resources. Its purpose is to define routing cluster-external requests to cluster-internal services. An Ingress will map URLs (hostname and path) to cluster-internal services.

# Ingress Controller
The Ingress is the definition of how the routing should be done. But the execution of those rules has to be performed by an “Ingress Controller”. Due to this, creating Ingress resources in a Kubernetes cluster won’t have any effect until an Ingress Controller is available.

The Ingress Controller is responsible of routing requests to the appropriate services within the Kubernetes cluster. How an Ingress Controller executes this task, is not explicitly defined by Kubernetes. Thus an Ingress Controller can handle requests in a way that works best for the cluster.

The Ingress Controller is responsible to monitor the Kubernetes cluster for any new Ingress resources. Based on the Ingress resource, the Ingress Controller will setup the required infrastructure to route requests accordingly.


# Key Components of Ingress Controllers
- Ingress Resource: This is a Kubernetes API object that defines how external HTTP/S traffic should be processed, including rules for routing to different services.
- Ingress Controller: The actual implementation of the Ingress resource. It can be implemented using various technologies like Nginx, Traefik, or HAProxy, each offering unique features and capabilities.
- Load Balancer: Often, cloud providers offer load balancing services that work in tandem with Ingress Controllers to distribute incoming traffic across multiple pods of a service.
- Rules and Paths: In the Ingress resource, rules and paths are defined to specify how different requests should be directed to different services. This includes setting up routing based on paths, domains, or header values.

# How Ingress Controllers Work
Request Flow in Kubernetes Ingress
Understanding the flow of an incoming request through the Ingress system is crucial for grasping the role of Ingress Controllers. Here's a simplified overview:

- Ingress Resource Creation: A user defines an Ingress resource, specifying rules for routing traffic to different services.
- Ingress Controller Watches for Changes: The Ingress Controller continuously monitors the Kubernetes API for changes in Ingress resources.
- Configuration Update: When a new Ingress resource is created or an existing one is modified, the Ingress Controller updates its configuration accordingly.
- Load Balancer Configuration (if applicable): In a cloud environment, the Ingress Controller may interact with the cloud provider's load balancer service to update the routing rules.
- Routing to Services: Incoming requests are directed to the appropriate service based on the rules defined in the Ingress resource.



# Routing Based ingress- nginx ingress

Routing is like the roadmap that directs incoming traffic to the right destinations within your cluster. With NGINX Ingress, you have a powerful tool at your disposal to customize and optimize this routing process according to your application’s needs.

So, what types of routing can we expect to encounter? Here’s a sneak peek:

- Basic Routing
- Path-Based Routing
- Host-Based Routing
- Wildcard Routing


- Basic Routing :- 
Basic routing ensures incoming traffic is directed to the appropriate backend services based on predefined rules. For example, you may have multiple replicas of a service and want to distribute traffic evenly among them. NGINX Ingress simplifies this process by allowing you to define rules for load-balancing across replicas or routing traffic based on port numbers.
Eg.day6 

- Host-based Routing :- 
Host-based routing directs traffic based on domain names. For example, let’s say you have two services: “blog” and “stream.” You want traffic from “blog.example.com” to go to the blog service and traffic from “stream.example.com” to go to the stream service. With NGINX Ingress, you can configure rules like this to route traffic based on the hostname specified in the request.
Eg. day7


- Path-Based Routing :- 
Sometimes, you need to direct traffic based on specific paths within your URLs. Path-based routing enables you to do just that. For example, you might want requests to “/blog” to go to blogging-svc service and requests to “/stream” to go to streaming-svc service. NGINX Ingress makes it easy to configure such rules, ensuring requests reach their intended destinations within your cluster.
Eg. day8

- Wildcard Routing :- 
Wildcard routing provides a flexible way to handle dynamic subdomains or paths within your URLs. For instance, you might want to route all requests for subdomains like “.example.com” to a particular service. By using wildcard characters like “ * ”, NGINX Ingress allows you to create rules that match a variety of URL patterns, providing more versatile routing configurations.
Eg. day9

# How to Check the pod information 
  kubectl get pods -v=7  [If you want more information about pods you can do 9 which is the maximum value of verbose]


# How to install nginx ingress controller in minikube
  minikube addons enable ingress

# Verify that the NGINX Ingress controller is running
  kubectl get pods -n ingress-nginx

# How to check the logs of ingress controller
  kubectl logs ingress-nginx-controller-768f948f8f-nlvgn -n ingress-nginx

# How to delete ingress resources 
  kubectl delete ing resource_name

# To see nginx ingress controller configuration
  kubectl exec -it -n ingress-nginx name_of_controller cat /etc/nginx/nginx.conf 