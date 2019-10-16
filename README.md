AWS ECS Fargate integration with AWS code pipeline.
POC by Venkatesh Singu (1066099)

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


Steps to follow:


a)	Go to ec2 service Create Target group:
 


b)	Create Load Balancer:
ALB listen on 80 - Tarketgroup listen on 8080 - ECS tasks run on 8080
•	Choose Application Load Balancer
•	
 
 

 
 
 
Register, Review and create.
 


c)	Go to ECR service in AWS - Create repository in ECR.

d)	Go to ECS service in AWS - Create cluster - choose ECS Fargate.
  
 

After creating a cluster. Click on View cluster.

e)	Create Task definition: In order to run containers inside this cluster, we need to have task definition.
•	Click on Create new task definition 
•	Select Launch type as FARGATE
•	Configure launch and container definitions.







 


 
 

 
After clicking on Add container: copy the ECR repository URL which you created earlier steps paste under Image:


 
 

Remaining things we can mark it as default for this demonstration. Click on Add.
Later click on Create.
 
We have a cluster and Task definition.
f)	Create a service under cluster.

 

Using service, we are going to create multiple tasks(replicas) And I want to add those tasks behind ALB such that end customers will hit ALB. ALB distributes traffic across multiple replicas of tasks.

Go to cluster -> open your cluster (which you created prior) -> under services click on create
 
 

 
Edit Security groups: Choose Type Custom TCP 

 




  

Click on Add to Load balancer ->
 
 

Remaining things we can mark it as default for this demonstration. Click on Add.
 
Review and create:
 

Clone repository into local where your source code is residing:
 
•	you need to clone GitHub repository (src code) into local, follow below steps:   git clone repo-url
aws configure (provide required values)
•	Go to ECR repository -> Click on View push commands -> execute those commands in Local.
 



After executing commands successfully, you can see below screen:
 


Goto Load Balancer-> copy the DNS name and paste in the browser.
 

 

************Application is successfully deployed on ECS Fargate******************






CI/CD implementation:
1.	Go to Code Build -> Create a build project and connect to Git hub 

 
 

 
 
 
 

Remaining things we can mark it as default for this demonstration. 
And Later we need to attach policy (AmazonEC2ContainerRegistryFullAccess) for role:  codebuild-Test-service-role-7890 
Go to roles:  search for codebuild-Test-service-role-7890
Attach policy: AmazonEC2ContainerRegistryFullAccess.

Click on Create Build project.

After Build get successful, Create code -pipeline.



 
 
Connect to GitHub by providing valid credentials.

  

 
 
Next -> Create pipeline:
Go to Load Balancer-> copy the DNS name and paste in the browser.
 

 

************Application is successfully deployed on ECS Fargate using CI/CD******************



