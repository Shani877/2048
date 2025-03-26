=== Kubernetes End to End Project on EKS(2048) ===

Note: I am using amazon machine with IAM role (AdminAccess policy).
Prerequisites
kubectl
eksctl 
AWS CLI 

Step-1. Tools Installation

Installation Kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client

Installation eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

Install aws cli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
aws configure #put your access key and secret access key

Step-2. Set up Kubernetes Cluster

Install IAM-authenticator 
curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
chmod +x ./aws-iam-authenticator
mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
aws-iam-authenticator help

eksctl create cluster \
  --name my-cluster \  # Change 'my-cluster' to your desired cluster name
  --node-type t2.medium \  # Change instance type as per your requirement (e.g., t3.medium, m5.large)
  --nodes 2 \  # Set the default number of worker nodes
  --nodes-min 1 \  # Minimum number of nodes in the cluster
  --nodes-max 3 \  # Maximum number of nodes in the cluster (for auto-scaling)
  --region us-east-1  # Change 'us-east-1' to your preferred AWS region (e.g., us-west-2, ap-south-1)

Step-3. Create pod and service yaml file and apply #I have already createdyaml files in my repo.
kubectl apply -f pod.yaml #apply pod file
kubectl get pods #check the pod

kubectl apply -f service.yaml #apply service file
kubectl get svc #check service

Step-4. Access the LoadBalancer service
curl <LoadBalancer_DNS:<Port_number> #on terminal
<LoadBalancer_DNS:<Port_number> #on browser







