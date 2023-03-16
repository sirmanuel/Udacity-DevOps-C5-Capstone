# Udacity-DevOps-C5-Capstone

## Application

It is a simple Spring Boot application.  
See: https://www.jetbrains.com/help/idea/your-first-spring-application.htm

Endpoints:
- /hello?myName=[yourName]
- /ping
- /actuator

## Infrastructure 
all Scripts for creating the in are in the folder /infra  


### Setup the Infrastructure
1. Create the VPC `make deploy-vpc`  
2. Create EKS Cluster `make deploy-eks`


### Cleanup Infrastructure

1. Delete the VPC `make delete-vpc`
2. Delete EKS Cluster `make delete-eks`

### Documentation of EKS
- Getting started: https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html
- Creating an Amazon EKS cluster: https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html
- Creating a VPC for your Amazon EKS cluster: https://docs.aws.amazon.com/eks/latest/userguide/creating-a-vpc.html

### add cluster

add konfiguration via eks (us separate konfig: config-demo)
https://docs.aws.amazon.com/cli/latest/reference/eks/update-kubeconfig.html
`aws eks update-kubeconfig --name udacity-c5-capstone-cluster --profile <your-profile> --region <your-region>`
`aws eks update-kubeconfig --name udacity-c5-capstone-cluster --region us-east-1 --kubeconfig=config-demo` 

view config
kubectl config --kubeconfig=config-demo view

(optional) set the current context (not necessary if only one context exists)
kubectl config --kubeconfig=config-demo use-context dev-frontend

list services
kubectl get svc --kubeconfig=config-demo

