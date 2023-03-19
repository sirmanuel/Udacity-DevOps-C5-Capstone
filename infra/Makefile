eks_clustername := udacity-devops-c5-capstone-cluster
eks_vpc_stackname := udacity-c5-capstone-eks-vpc
eks_vpc_template_file := eks-vpc-private-subnets.yaml
aws_region_code := us-east-1
aws_profile := default
app_name := spring_demo


hello:
	echo "Hello, World"

deploy-eks:
	eksctl create cluster --name $(eks_clustername) \
	--version 1.25 \
	--region $(aws_region_code) \
	--nodegroup-name linux-node \
	--node-type t2.medium --nodes 2 \
	--profile $(aws_profile)

delete-eks:
	eksctl delete cluster --region=$(aws_region_code) --name=$(eks_clustername) --profile $(aws_profile)

deploy-ecr:
	aws ecr create-repository \
    --repository-name $(app_name) \
    --image-scanning-configuration scanOnPush=true \
    --region $(aws_region_code) \
	--profile $(aws_profile)

delete-ecr:
	aws ecr delete-repository \
	--repository-name $(app_name) \
    --region $(aws_region_code) \
	--force \
	--profile $(aws_profile)


## The following commands are in process and not part of this project yet
deploy-vpc:
	@echo 'eks stack name: $(eks_vpc_stackname)'
	aws cloudformation deploy --stack-name $(eks_vpc_stackname) \
	--template-file $(eks_vpc_template_file) \
	--capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM" \
	--region=$(aws_region_code) \
	--profile $(aws_profile)
	
delete-vpc:
	aws cloudformation delete-stack --stack-name $(eks_vpc_stackname) \
	--region $(aws_region_code)


deploy-eks2:
	PRIV_SUBNETS=$(shell make q_priv-subnets); \
	\
	eksctl create cluster \
	--name $(eks_clustername) \
	--region $(aws_region_code) \
	--version 1.25 \
	--vpc-private-subnets $$PRIV_SUBNETS \
	--without-nodegroup \
	--profile $(aws_profile)

q_priv-subnets:
	@aws cloudformation describe-stacks --stack-name $(eks_vpc_stackname) --no-paginate \
	--query "Stacks[*].Outputs[?OutputKey=='PrivateSubnets'].OutputValue" --output text \
	--profile $(aws_profile)
