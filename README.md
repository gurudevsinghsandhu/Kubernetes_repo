Kubernetes Concepts: K8s

# Kubernetes Architecture:- 


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