Kubernetes-Zero-to-Hero
Creating this repo with an intent to make Kubernetes easy for begineers. This is a work-in-progress repo.

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


[def]: <kubernetes architecture.jfif>
[def2]: <kubernetes architecture-1.jfif>
