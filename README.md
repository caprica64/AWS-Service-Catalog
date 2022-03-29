# AWS-Service-Catalog

This project is to design an AWS Shared Service Catalog proof of concept where a central Portfolio (DevTools) account has sets of portfolios and products. Those portfolios should be shared with an AWS Organization OU that contains DevOps accounts for projects.

These projects will have two or more product environment accounts where products are deployed, such as VPC, network routes and subnets, ECS and others. When you deploy an application service such as an ECS Service/Task, it should also deploy the pipeline for versioning those workloads.

## Service Catalog with multiple products.

This project repository has examples of CloudFormation templates that can be used to set up portfolios and related products as building blocks to self-serving teams using AWS accounts. The goal here is demonstrate how one service output can be reused into another component to create a workload. For example, a **VPC** product from a Foundation portfolio can be used as an input for NAT Gateway product to enable outbound internet access to it and also be used as an input for an **ECS cluster** product. The very same ECS Cluster can be reused as the home cluster of one or more products such as Fargate Services.

AWS Service Catalog project to set up basic VPC and a DevOps pipeline. Initial setup will allow the sequencial use of CloudFormation templates to create a VPC, Network Gateway with updated routes, than one ECS Cluster with an ALB and later multiple ECS Services.

The final goal is to be able to build versioned products that will be made available in the Service Catalog to the approved AWS Organizations users. This project only covers the product relationship between them. Please, see the https://github.com/caprica64/AWS-Service-Catalog-Versioned-Product as a guide to build Service Catalog demo project for a single AWS account using the templates here.

## Instructions

Those templates are numbered in the order they are set up. The VPC is the first product and the NAT Gateway template depends on the VPC stack name to attach it and modify the route tables. 

Once you have VPC and NAT Gateways in place, you can deploy the ECS cluster with will set up an ALB and security group for internet access. Than, launch the forth template for one or more ECR repositories. Note that those will be empty of Docker container images at the begining. You need to upload some kind of Hello World to test and complete the fifth stack for the ECS Services.

**Important**: The ECS template creates a **Certificate Manager** certificate for the ALB using Route 53 public hosted zone, which should be already created. In the future, we should have a template product in foundation to create a hosted zone. 

### How to create a Hello World container
**Pending to write instructions on how to upload a single app such as an NGINX site**

On the fifth template for an ECS Service, please make sure you use both VPC, Repo and ECS Cluster parameters as stacknames so it can import the parameters properly. The design goal here is to be able to import as much parameters as possible and make simple decisions for end users. It is a trade-off if the consumer want to customise some values. Maybe an organization can deploy similar products with flexible parameters or have more complex templates with conditions that the consumer set to reuse standard values or custom ones. 

**Important**: when creating a second or multiple services, please add a differente service order or priority parameter, otherwise the stack will fail. Since the ALB listener used multiple service hostnames (instead of pathname or routes), this is a requirement. An improvement to the project is enabling different /routes per workload.

Once you launch the ECS Service template it should build Service, Task, Target Groups, Security Group for the Service and attach the target group to the cluster ALB. The project assumes you will be using a single ALB with a hosted zone. Also, you can define a health check path different from the root site path in your tests.

Once you are done with testing, it is necessary to clean up ECR repository before deleting the stack. You should delete all the stacks in the reverse order.

We are not covering a pipeline setup for the product yet, but it is possible to build one in AWS using CodeCommit for the repository, CodePipeline for the pipeline, CodeBuild for the container build and repository update and ECS as the target deploy stage. Going further it should be possible to add code and container verification stages, i.e DAST/SAST products.

- 2022-03-29 > There is no service autoscaling yet.
- 2022-03-29 > There is no autoprovisioning of a sample docker container to quick test. Some automation could take the service port number, use it on an NGINX config file and docker build a hello world container.

## Change log

- 2022-02-26 > Added vpc.yaml intially tested
- 2022-03-13 > Added NAT Gateway template, tested Service Catalog. Added ECS/ALB Stack and added Service/Task stack.
- 2022-03-14 > Moved ALB Listener from Services stack into the ALB one, since the listener is shared between several services using same port
- 2022-03-14 > Finished v1.0.0 by taking all parameters out. Pending now, add ECR repo code within the Service stack.
- 2022-03-15 > Include new ECR repo code in the Service/Task code. Added imported references. Added a Health Check parameter.
- 2022-03-16 > Added an export stack name into the stacks so they can be looked and reused. Tested all 5 stacks in Service Catalog.
	- Create new services work alright for one service. Need to test with multiple services.
	- ECR stack fails termination because ECR won't delete a repo that is not empty. Probably will require a maintain policy, similar to S3.
	- Still, we need to be able to register the services into a Route 53 record in the same account. For the future into a R53 in another account. Consider using Conditions and Parameters for the consumer to choose.
- 2022-03-29 > Updated README.

