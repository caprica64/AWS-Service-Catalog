# AWS-Service-Catalog
AWS Service Catalog project to set up basic VPC and a DevOps pipeline. Initial setup will allow the sequencial use of CloudFormation templates to create a VPC, Network Gateway with updated routes, than one ECS Cluster with an ALB and later multiple ECS Services.

- 2022-02-26 > Added vpc.yaml intially tested
- 2022-03-13 > Added NAT Gateway template, tested Service Catalog. Added ECS/ALB Stack and added Service/Task stack.
- 2022-03-14 > Moved ALB Listener from Services stack into the ALB one, since the listener is shared between several services using same port
- 2022-03-14 > Finished v1.0.0 by taking all parameters out. Pending now, add ECR repo code within the Service stack.
- 2022-03-15 > Include new ECR repo code in the Service/Task code. Added imported references. Added a Health Check parameter.
- 2022-03-16 > Added an export stack name into the stacks so they can be looked and reused. Tested all 5 stacks in Service Catalog.
	- Create new services work alright for one service. Need to test with multiple services.
	- ECR stack fails termination because ECR won't delete a repo that is not empty. Probably will require a maintain policy, similar to S3.
	- Still, we need to be able to register the services into a Route 53 record in the same account. For the future into a R53 in another account. Consider using Conditions and Parameters for the consumer to choose.
- 2022-03-17 > 

