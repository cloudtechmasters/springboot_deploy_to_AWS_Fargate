# springboot_deploy_to_AWS_Fargate
Deploy springboot Application to AWS Fargate


# Springboot_app_deploy_to_AWS_ECS_cluster
Deploy a Spring Boot App on AWS ECS Cluster


Introduction

Amazon Elastic Container Service is a managed container orchestration service which allows you to deploy and scale containerized applications. An overview of the features and pricing can be found at the [AWS website.](https://aws.amazon.com/ecs)

**ECS consists out of a few components:**

- Elastic Container Repository (ECR): A Docker repository to store your Docker images (similar as DockerHub but now provisioned by AWS).
- Task Definition: A versioned template of a task which you would like to run. Here you will specify the Docker image to be used, memory, CPU, etc. for your container.
- ECS Cluster: The Cluster definition itself where you will specify how many instances you would like to have and how it should scale.
- Service: Based on a Task Definition, you will deploy the container by means of a Service into your Cluster.

1. Create the App

https://github.com/tushardashpute/springboohello-CICD.git

Run the build in order to create the jar file 

mvn clean install

2. Upload Image to ECR

Navigate in AWS to the ECS Service and select in the left menu the Repositories section. First thing to do, is to create a repository by clicking the Create repository button. In order to see how you can push the Docker image to the repository, click the View push commands button which is available in the repository overview.

aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin ***********.dkr.ecr.us-east-2.amazonaws.com

docker build -t springdemo .

docker tag springdemo:latest ***********..dkr.ecr.us-east-2.amazonaws.com/springdemo:latest

docker push ***********..dkr.ecr.us-east-2.amazonaws.com/springdemo:latest

After successful upload, the Docker image is added to the repository.

<img width="1237" alt="image" src="https://user-images.githubusercontent.com/74225291/180633098-88456c6b-42af-4120-9d0d-21687c2482d4.png">


3. Create Task Definition

Now that the Docker image is available in ECR, next thing to do is to create a Task Definition by creating the Create new Task Definition button in the Task Definitions section (left menu).

<img width="1481" alt="image" src="https://user-images.githubusercontent.com/74225291/180633161-efaf379c-33fe-4a85-9d5b-5e7f483c7297.png">

In step 1, choose for an EC2 self managed task and click the Next step button.

750285159662.dkr.ecr.us-east-2.amazonaws.com/springdemo:latest

<img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/180640675-c70b2b38-fe73-4ace-84f3-b7d312ec21e9.png">

<img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/180640692-f38c51d7-f636-4365-acb9-b6e5f27185f1.png">

<img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/180640744-6c5057bc-37cb-4358-b0ff-0bedb36620a8.png">

Click Next

Now Review and Create the task defination.

<img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/180640627-84a9a0f3-0060-45eb-8060-de14f8d1af67.png">

4. Create Cluster

Navigate in the left menu to the Clusters section and click the Create cluster button.

<img width="901" alt="image" src="https://user-images.githubusercontent.com/74225291/180641189-effdaadb-497e-45f7-bb0f-f5264d70c0f4.png">

<img width="880" alt="image" src="https://user-images.githubusercontent.com/74225291/180641220-1f012e38-b9c7-4ddc-a1ee-08275aa36826.png">

Now click on create cluster.

5. Create Service

Time to tie everything together, so let’s create the Service in the AWS Fargate cluster in order to run the Docker image. Navigate to the cluster and click the Deploy button in the Services tab.

<img width="942" alt="image" src="https://user-images.githubusercontent.com/74225291/180643072-cc730896-c9f2-42c4-b770-766e9100c337.png">

Time to tie everything together, so let’s create the Service in the AWS Fargate cluster in order to run the Docker image. Navigate to the cluster and click the Create button in the Services tab.

<img width="983" alt="image" src="https://user-images.githubusercontent.com/74225291/180640567-54883c3c-bb31-4a66-870d-7a22f700893f.png">

<img width="967" alt="image" src="https://user-images.githubusercontent.com/74225291/180643094-e3622369-3857-41be-9464-33e44057bb97.png">

<img width="867" alt="image" src="https://user-images.githubusercontent.com/74225291/180643131-9b10493c-6e01-4b3a-b005-1395678e3a81.png">

<img width="898" alt="image" src="https://user-images.githubusercontent.com/74225291/180643177-116364bf-19b2-45b3-b3c8-6b663317fc74.png">

Click on Deploy.

Once task is deploy you will see ALB is created. Goto LoadBalancer mand make sure to white list port 33333 in SG for ALB.

<img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/180643244-7006f0f0-1e1c-4a7c-88fb-f7a7570bfc78.png">

<img width="1248" alt="image" src="https://user-images.githubusercontent.com/74225291/180643392-a9eaf7b1-a65d-4577-b87a-239fe502fa5a.png">

<img width="1292" alt="image" src="https://user-images.githubusercontent.com/74225291/180643450-d3813c88-5bf7-4612-a6bc-aaacd04f4d9b.png">

Now access the Sprinboot Application deployed on ECS Fargate using the ALB DNS as highlighted above.

<img width="942" alt="image" src="https://user-images.githubusercontent.com/74225291/180643492-e2d562cd-f720-44d4-9b4d-19c026473bd9.png">

