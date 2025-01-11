# Kubernetes_Concepts: K8s

How to install Minikube using chocolatey on Window:-

To install :-
choco install minikube

Start your cluster :- [ Make sure docker would be installed in your system
minikube start --driver=docker

Install Kubectl command using curl :-
Firstly install curl using chocolatey after that install curl using choco 

If you have curl installed, use this command:
curl.exe -LO "https://dl.k8s.io/release/v1.32.0/bin/windows/amd64/kubectl.exe"

Validate the binary :- 
curl.exe -LO "https://dl.k8s.io/v1.32.0/bin/windows/amd64/kubectl.exe.sha256"

Append or prepend the kubectl binary folder to your PATH environment variable:-

Test to ensure the version of kubectl is the same as downloaded:-
kubectl version --client

# To access your minikube cluster for accessing your application {curl Pod_ip}
minikube ssh

curl pod_ip


# For Creating a Container: - 
   kubectl create -f pod.yml

# To check the pod
   kubect get pods 

# To check the more details about pods 
   kubectl get pods -o wide

Note:- To check all the command check kubectlcheatsheat on google

# To Delete the Pod
   kubectl delete pod nginx 


# How to list all the resource in a perticular namespace 
   kubectl get all

# To list all the resources from all the namespaces 
   kubectl get all -A

# To List Deployment in k8s 
   kubectl get deployment

# To list ReplicaSet
   kubectl get rs 

# To watch the pods 
   kubectl get pods -w

# IF you done few changes inside your deployment file and now you want to apply them you need to run this command
   kubectl apply -f filename.yml 