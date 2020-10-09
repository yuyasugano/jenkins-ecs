## CloudFormation example for Jenkins on ECS

This example show how to set up Jenkins on AWS ECS by using `CloudFormation`. 

1. Deploy underlying VPC, subnets, IGW, NAT gateways and related components
2. Full fleged Jenkins image with required plugins will be hosted in Fargate ECS
3. Application Load Balancer monitors the ECS service and run application
 
## aws cli version

Ensure your `aws cli` version is updated or satisfy the version I testes as follows:
```sh
aws --version
aws-cli/1.16.212 Python/2.7.12 Linux/4.4.0-62-generic botocore/1.12.202
```
To get details `CloudFormation`, visit https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html

## Jenkins plugins
 
amazon-ecs:1.37
git:4.4.4
git-client:3.5.1
greenballs:1.15
 
## Setup steps

1. Upload `default-vpc.yaml` to S3 bucket and replace VPCStack's template url with S3 url
2. Add required plugins for Jenkins in plugins.txt and build Docker image
3. Push the built Docker image to Dockerhub or ECR depending on your choice
4. Replace Image name in ContainerDefinitions of JenkinsTaskDefinition in `jenkins-ecs.yaml`
5. Upload `jenkins-ecs.yaml` to S3 bucket and run the following command with given S3 url
```sh
aws cloudformation create-stack --stack-name jenkins-ecs --template-url <s3 url> --capabilities "CAPABILITY_NAMED_IAM" "CAPABILITY_AUTO_EXPAND"
```
 
## Clean up

```sh
aws cloudformation delete-stack --stack-name jenkins-ecs
```

## License
 
This code is licensed under the MIT License.
