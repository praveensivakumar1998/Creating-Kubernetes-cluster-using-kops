# Creating-Kubernetes-cluster-using-kops

# What is KOps?
kOps, also known as Kubernetes operations, is an open-source project which helps you create, destroy, upgrade, and maintain a highly available, production-grade Kubernetes cluster. Depending on the requirement, kOps can also provision cloud infrastructure.

## Kubernetes Installation using KOps on ubuntu based EC2 machine

### Requirements:
1. AWSCLI
2. kubectl

## KOps Setup Prerequisites:
### Following are the prerequisites for KOps Kubernetes cluster setup.
1. EC2 machine with Ubuntu 2022 Operating System based 
2. IAM Role with following permissions with Use case as EC2
    - AmazonEC2FullAccess
    - AmazonRoute53FullAccess
    - AmazonS3FullAccess
    - IAMFullAccess
    - AmazonVPCFullAccess
    - AmazonSQSFullAccess
    - AmazonEventBridgeFullAccess
3. S3 Bucket for store state (eg: k8straining.k8s.local-storage) with versioning enabled

  ![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/70196bfd-dd97-4817-bbd3-d4fb3e0e8b73)

### Install AWSCLI
```
sudo apt update
sudo hostnamectl set-hostname k8s-kOps
sudo apt install awscli -y
```
### Install Kubectl
ref: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```
### Install KOps:
```
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/kops
```
### Validating a Installed packages
```
aws --version
kubectl version --client
kops version
```
![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/a706d359-a35d-4194-b73a-91779e5963a5)

### Creating your cluster

```
export KOPS_STATE_STORE=s3://k8straining.k8s.local-storage
```
The command export **KOPS_STATE_STORE=s3://prefix-example-com-state-store** is used to define the state storage location for kops, which is a tool used to manage Kubernetes clusters on various cloud platforms, including AWS (Amazon Web Services).

```
 kops create cluster --name=k8straining.k8s.local --zones=ap-south-1a --node-count=1 --node-size=t3.small --master-size=t3.small --node-volume-size=10 --master-volume-size=15 --yes
```
Now the Cluster creating would be done,
![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/867b7c17-1681-40e4-a9d7-782ffadd406a)

above cmd will create a seperate **VPC, subnets, security groups, autoscaling groups and loadbalancer**

![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/7a712bcf-95ab-4a58-ad36-89a2db4dfb9c)

### Update and Validating a cluster
**Update the cluster nodes**
```
kops update cluster --name k8straining.k8s.local --yes
```
![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/8255a7d8-dc22-4d59-a684-f3a68ef2d37d)

**Validate the cluster components**
```
kubectl get nodes
```
![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/43851424-1d4e-403d-986d-af0496da245c)

```
kops validate cluster
```
![image](https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops/assets/108512714/3388fe44-2558-4c6f-a71d-acc56a2dff1e)


Now the Kubernetes cluster creating would be done. 
