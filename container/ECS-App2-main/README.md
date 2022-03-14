# ECS-App1
Simple repository to test a web app in AWS ECS, this will hold a web page file to test deployment through CodePipeline, CodeBuild and use Cloud9 as IDE. The repo will hold a simple **buildspec.yml** file to hold container build script to push image to a ECR repository.

2022-02-19 > Added a HTTPS proxy statement to buildspec.yml and added necessary routes to the VPC to peer with another one with a proxy squid instance.
2022-03-14 > Changed the service name for a new lab. This file was taken from a previous project.


