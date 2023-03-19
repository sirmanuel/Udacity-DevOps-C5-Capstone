# Udacity-DevOps-C5-Capstone
This Project use a CICD of CIRCLECI to deploy a Java Application to AWS EKS.
The deployment is configured as a rolling deployment defined in the k8s.yaml.
The app version based on the version number in the pom file.

### Further readimgs
- Getting started: https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html

## Application
It is a simple Spring Boot application.  
See: https://www.jetbrains.com/help/idea/your-first-spring-application.htm

### Prerequires
Java 17 JDK
Maven 3.8.1 (https://maven.apache.org/download.cgi)

### run Appplication
1. compile: `mvn package`
2. run: `java -jar ./target/*.jar`

### Endpoints
By default you can reach the Application on `localhost:8080/`

Frontend (simple HTML page):
- /

API Endpoints
- /hello?myName=[yourName]
- /ping
- /actuator

## Docker
### Prerequires
Docker (https://docs.docker.com/engine/install/#installation)

### Commands
- Build docker image: `docker build --build-arg JAR_FILE=/target/*.jar -t spring_demo .`
- show existing images: `docker images`
- Run docker image locally: `docker run spring_demo -p 8080:8080`
  - The application is reachanle on: `localhost:8080/`

## Circle CI
Please watch the [video walkthrough of setting up your secrets here](https://www.youtube.com/watch?v=caFJQ1YwVdU).

1. Add the following environment variables to your Circle CI project by navigating to {project name} > Settings > Environment Variables as shown [here](https://circleci.com/docs/2.0/settings/):
  - `AWS_ACCESS_KEY_ID`=(from IAM user with programmatic access)
  - `AWS_SECRET_ACCESS_KEY`= (from IAM user with programmatic access)
  - `AWS_REGION`=(the used region in aws - for this example 'us-east-1')
  - `AWS_ECR_REPOSITORY`=(the name of the ECR repository - for this example 'spring_demo')
  - `AWS_ECR_REGISTRY_ID`=(The 12 digit AWS id associated with the ECR account. This field is required)

## Project Infrastructure
For th CICD Pipeline an EKS cluster and an ECR repository is necessary.

### Prerequires
AWS CLI (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
eksctl (https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

### Setup the Infrastructure AWS
1. Create EKS Cluster `make deploy-eks`
   - Known issues: us-east-1e, the targeted availability zone, does not currently have sufficient capacity to support the cluster. If this error is thrown, cleanup and redeploy the eks cluster.
2. Create ECR Repository `make deploy-ecr`

### Cleanup Infrastructure
1. Delete EKS Cluster `make delete-eks`
2. Delete ECR Repository `make delete-ecr`

## Push Image to AWS ECR
### Prerequires
- ECR repository exists.
- Dockerfile is created locally.

### Commands
See instructuions in ECR for the created repository.  
(Alternatively see AWS documentation: https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)

## Connect to EKS local
### Prerequires
kubectl (https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

### Commands
- load konfiguration of the cluster to communicate with: `aws eks update-kubeconfig --name udacity-devops-c5-capstone-cluster --profile default --region us-east-1`
- view config: `kubectl config view`
- set current context to communicate with: `kubectl config use-context <context-name>` (get context-name from config file)
- list K8s services `kubectl get svc`

## Deploy to EKS
In the file k8s.yaml you can find the definition of the service and the deployment.
In the pipline some variables will be replaced. If you use this definition locally, please replace the variables

### Prerequires
- EKS Cluster exists.
- kubectl is configured for the cluster.
- A Dockerfile must be uploaded to ECR or another image repository.
- Update k8s.yaml with the image name.

### Commands
execute: `kubectl apply -f k8s.yaml`
