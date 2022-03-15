# AWS-Service-Catalog
AWS Service Catalog project to set up basic VPC and a DevOps pipeline. Initial setup will allow the sequencial use of CloudFormation templates to create a VPC, Network Gateway with updated routes, than one ECS Cluster with an ALB and later multiple ECS Services.

- 2022-02-26 > Added vpc.yaml intially tested
- 2022-03-13 > Added NAT Gateway template, tested Service Catalog. Added ECS/ALB Stack and added Service/Task stack.
- 2022-03-14 > Moved ALB Listener from Services stack into the ALB one, since the listener is shared between several services using same port
- 2022-03-14 > Finished v1.0.0 by taking all parameters out. Pending now, add ECR repo code within the Service stack.
- 2022-03-15 > Include new ECR repo code in the Service/Task code. Added imported references. Added a Health Check parameter.

