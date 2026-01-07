# Kubernetes on AWS (EKS) – Deploy a Containerized Application

## 📌 Project Overview
This project demonstrates how to deploy a containerized web application on **Amazon EKS (Elastic Kubernetes Service)** using Docker, Amazon ECR, and Kubernetes.

The application is deployed with high availability using multiple pods and exposed to the internet using an AWS LoadBalancer.

---

## 🛠️ Technologies Used
- AWS EKS
- AWS ECR
- Docker
- Kubernetes
- eksctl
- kubectl
- NGINX

---

## 🏗️ Architecture
1. Docker image is built locally
2. Image is pushed to Amazon ECR
3. EKS cluster is created using eksctl
4. Application is deployed using Kubernetes Deployment
5. Application is exposed using Kubernetes LoadBalancer Service

---

## ✅ Prerequisites
- AWS account
- IAM user with required permissions
- AWS CLI installed and configured
- Docker installed
- kubectl installed
- eksctl installed

## 🚀 Step-by-Step Implementation

### Step 1: Configure AWS CLI
- Configure AWS CLI using IAM user credentials
- Set default region
 - Configure AWS CLI using IAM user credentials and set default region.
aws configure
aws configure get region
aws sts get-caller-identity
### Step 2: Create EKS Cluster -
Create EKS cluster using eksctl
- Use existing VPC and public subnets
- Verify cluster creation
- Create an Amazon EKS cluster using eksctl with existing public subnets.
eksctl create cluster \
--name my-eks-cluster \
--region ap-south-1 \
--vpc-public-subnets <subnet-id-1>,<subnet-id-2> \
--nodegroup-name my-nodes \
--node-type t3.medium \
--nodes 2

- Verify cluster creation:
eksctl get cluster --region ap-south-1

### Step 3: Configure kubectl
 - Update kubeconfig
 - Verify worker nodes using kubectl get nodes
 - Update kubeconfig and verify worker nodes
aws eks update-kubeconfig --region ap-south-1 --name my-eks-cluster

kubectl get nodes

### Step 4: Create Application Files 
- Create a simple HTML file
- Create Dockerfile for NGINX-based container
- Create application directory and required files.
  mkdir eks-app
  cd eks-app
- Create files:
  index.html
  Dockerfile
### Step 5: Build Docker Image
   Build Docker image locally
   Verify image creation
- Build Docker image locally and verify.
docker build -t eks-app .
docker images

### Step 6: Push Image to Amazon ECR 
- Create ECR repository
- Authenticate Docker with ECR
- Tag and push Docker image to ECR
  
- Create ECR repository and authenticate Docker:
  
aws ecr create-repository --repository-name eks-app --region ap-south-1

aws ecr get-login-password --region ap-south-1 | \

docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-south-1.amazonaws.com


Tag and push image:

- docker tag eks-app:latest <account-id>.dkr.ecr.ap-south-1.amazonaws.com/eks-app:latest
- docker push <account-id>.dkr.ecr.ap-south-1.amazonaws.com/eks-app:latest

### Step 7: Deploy Application on EKS 
- Create Kubernetes Deployment YAML
- Apply deployment using kube
- Verify running pods

-Create Kubernetes deployment and apply it:
kubectl apply -f deployment.yaml
kubectl get pods 

### Step 8: Expose Application 
- Create Kubernetes Service of type LoadBalancer -
-  Apply service
- Retrieve external LoadBalancer URL

-Create LoadBalancer service and retrieve external URL:
kubectl apply -f service.yaml
kubectl get svc eks-app-service

### Step 9: Access Application -
-Access application using external LoadBalancer DNS 
- Verify application response in browser

-Access application using the external LoadBalancer DNS.

http://<external-loadbalancer-dns>

Verify application response in browser.

### 🎯 Outcome

Application successfully deployed on Amazon EKS

High availability achieved using multiple pod replicas

Application accessible via public AWS LoadBalancer

### 🧹 Cleanup (Cost Optimization)

Delete Kubernetes resources and EKS cluster to avoid AWS billing.

- kubectl delete svc eks-app-service
- kubectl delete deployment eks-app
- eksctl delete cluster --name my-eks-cluster --region ap-south-1

### 📌 Conclusion
This project demonstrates a complete end-to-end deployment of a containerized application on Amazon EKS, covering Docker image creation, Amazon ECR integration, Kubernetes deployment, and LoadBalancer-based application exposure.
