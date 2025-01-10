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
