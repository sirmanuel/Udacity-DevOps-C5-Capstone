eks_clustername := udacity-c5-capstone-cluster
eks_vpc_stackname := udacity-c5-capstone-eks-vpc
eks_vpc_template_file := eks-vpc-private-subnets.yaml
aws_region_code := us-east-1


hello:
	echo "Hello, World"


deploy-vpc:
	@echo 'eks stack name: $(eks_vpc_stackname)'
	aws cloudformation deploy --stack-name $(eks_vpc_stackname) --template-file $(eks_vpc_template_file) --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM" --region=$(aws_region_code)
	
delete-vpc:
	aws cloudformation delete-stack --stack-name $(eks_vpc_stackname) --region $(aws_region_code)

deploy-eks:
	PRIV_SUBNETS=$(shell make q_priv-subnets); \
	\
	eksctl create cluster \
	--name $(eks_clustername) \
	--region $(aws_region_code) \
	--version 1.25 \
	--vpc-private-subnets $$PRIV_SUBNETS \
	--without-nodegroup

q_priv-subnets:
	@aws cloudformation describe-stacks --stack-name $(eks_vpc_stackname) --no-paginate \
	--query "Stacks[*].Outputs[?OutputKey=='PrivateSubnets'].OutputValue" --output text

delete-eks:
	eksctl delete cluster --region=$(aws_region_code) --name=$(eks_clustername)
	# known errors:
	# Error: cannot list Kubernetes Services: Unauthorized
    # make: *** [delete-eks] Error 1

