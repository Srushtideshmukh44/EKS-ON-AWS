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

## 📂 Project Structure
