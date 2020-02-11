AWS ECS Fargate integration with AWS code pipeline.
POC by Shivakarthik (1085346)

AWS Fargate is a compute engine for Amazon ECS that allows you to run containers without having to manage servers or clusters


prerequisites:
 
1.	AWS account, GitHub Account (code commit), Docker hub Account (ECR).
2.	Docker based application source code store either in GitHub or AWS code commit.
3.	Create Target group
4.	Create Load Balancer
5.	Create repository in ECR
6.	Create cluster
7.	Create Task definition
8.	Create a service under cluster.
9.	For CI/CD create code -build and code-pipeline


===================================================================
 Steps:
 
 a. Goto ECS - Create cluster - choose ECS Fargate
 b. Task definition: In order to run containers inside this cluster ,we need to have task definition.Inside the Task Definition add containers.
 c. Create ECR repo.
 d. Create Tarketgroup -- choose ip--port 8080
 e.	Create Load Balancer --Load balancer lisner for port 80.
 (A load balancer is a device that acts as a reverse proxy and distributes network or application traffic across a number of servers. )
	Load balancer protocal -HTTP -80--choose Availibility Zones
	default--create Security Groups--type HTTP--source custome-- create configure routing--choose existing  Tarketgroup (earlier created)
	ALB listen on 80 - Tarketgroup listen on 8080 - ECS tasks run on 8080
 f. Create a service under cluster.(service uses task definition to create containers) 	Launch type- Fargate sericeName- QAutoApp
	Number of tasks-3
	 Edit new security group- choose custome tcp-8080
	 choose Load Balancer type & Name (Add Load Balancer)
	 Production listener port -80:HTTP
	 Tarketgroup-existinggroup
	 
 g. login into ECR
 h. application succuessfully deployed on ecs fargate cluster
 i. CI/CD setup
 j. Goto code build -  (going to pull repo using buildspec.yml its going to build docker image and push this into ECR)
	
	1. update buildspec.yml -> REPOSITORY URL: 968350853817.dkr.ecr.us-east-2.amazonaws.com/node-app
	2. create build project
	3. attach policy(AmazonEC2ContainerRegistryFullAccess) for role:  codebuild-xyz-service-role-12345(goto role serach for codebuild-xyz-service-role-12345)
Note: We have complete setup for cluster,github, codebuild( pull src code from github and then build image and push to ecr).

k. code pipeline - Create code pipeline 
	
=================================================================================
************Application is successfully deployed on ECS Fargate using CI/CD*****************



