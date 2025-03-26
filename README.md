# Kubernetes End-to-End Project on EKS (2048 Game)

## Introduction
This project demonstrates how to deploy the 2048 game on an Amazon EKS (Elastic Kubernetes Service) cluster using Kubernetes.

## Prerequisites
Ensure you have the following tools installed on your Amazon EC2 instance (with an IAM role having **AdminAccess** policy):
- **kubectl** (Kubernetes CLI)
- **eksctl** (EKS cluster management tool)
- **AWS CLI** (To interact with AWS services)

## Step 1: Install Required Tools

### Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### Install eksctl
```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

### Install AWS CLI
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
```
_(Enter your AWS Access Key and Secret Access Key when prompted)_

## Step 2: Set Up Kubernetes Cluster

### Install IAM Authenticator
```bash
curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
chmod +x ./aws-iam-authenticator
mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
aws-iam-authenticator help
```

### Create EKS Cluster
```bash
eksctl create cluster \
  --name my-cluster \  # Change 'my-cluster' to your desired cluster name
  --node-type t2.medium \  # Change instance type as needed (e.g., t3.medium, m5.large)
  --nodes 2 \  # Default number of worker nodes
  --nodes-min 1 \  # Minimum number of nodes
  --nodes-max 3 \  # Maximum number of nodes (auto-scaling)
  --region us-east-1  # Change to your preferred AWS region
```

## Step 3: Deploy Application to EKS

### Apply Kubernetes YAML Files
_(Ensure you have the YAML files `pod.yaml` and `service.yaml` in your repository)_

```bash
kubectl apply -f pod.yaml  # Deploy the pod
kubectl get pods  # Verify pod status

kubectl apply -f service.yaml  # Deploy the service
kubectl get svc  # Verify service status
```

## Step 4: Access the Application

### Get the LoadBalancer URL
```bash
kubectl get svc
```
Copy the **EXTERNAL-IP** (LoadBalancer DNS) from the output and use it as shown below:

#### Access via Terminal
```bash
curl <LoadBalancer_DNS>:<Port_Number>
```

#### Access via Browser
Open the following in your browser:
```
http://<LoadBalancer_DNS>:<Port_Number>
```

## Conclusion
Successfully deployed the **2048 game** on an AWS EKS cluster using Kubernetes! 🚀

