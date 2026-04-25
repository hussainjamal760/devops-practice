This guide explains how to deploy a **Dockerized application** on AWS using:

- **IAM User & AWS CLI Configuration**
- **Amazon Elastic Container Service (ECS)**
- **Amazon Elastic Container Registry (ECR)**
- **Application Load Balancer (ALB)**
- **Security Groups**
- **Image Management**
- **Networking (VPC, Subnets, Target Groups)**

---
1. Creating an IAM User for AWS CLI Authentication
Before doing anything, we need an IAM User with the correct permissions.

Go to AWS IAM Console → Users → Click Create User.
Enter User Name: aws-ecs-user.
Select "Access Key - Programmatic Access" (needed for CLI access).
Click Next: Permissions → Select Attach Policies Directly.
Search for AmazonEC2ContainerRegistryFullAccess, select it, and click Next.
Click Create User.
Download the .csv file containing Access Key ID & Secret Access Key.
2. Configuring AWS CLI
Once the IAM user is created, configure the AWS CLI:

aws configure
You'll be prompted to enter:

AWS Access Key ID: (from .csv file)
AWS Secret Access Key: (from .csv file)
Default region name: (e.g., us-east-1, us-west-2)
Default output format: (leave blank or type json)
To verify if AWS CLI is correctly configured:

aws s3 ls
If no errors appear, your AWS CLI is ready.

3. Creating Security Groups
Security groups control incoming and outgoing traffic to ALB and ECS Tasks.

Go to AWS Console → EC2 Dashboard → Security Groups.

Click Create Security Group → Name it ALB-SG (for Load Balancer).

Inbound Rules:

Type: HTTP
Protocol: TCP
Port Range: 80
Source: 0.0.0.0/0 (Allow all)
Click Create Security Group.

Create another Security Group → Name it ECS-SG (for ECS tasks).

Inbound Rules:

Type: Custom TCP Rule
Protocol: TCP
Port Range: 3000
Source: ALB-SG (important: only ALB should access ECS tasks)
Click Create Security Group.

4. Creating an ECR Repository
Go to AWS Console → ECR → Create Repository.
Name the repository: my-app
Set visibility to Private.
Click Create.
Copy the ECR Repository URI, e.g.:
<aws-account-id>.dkr.ecr.<region>.amazonaws.com/my-app
5. Building & Pushing Docker Image to ECR
Authenticate Docker with ECR:

aws ecr get-login-password | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<region>.amazonaws.com
Build & tag the image:

docker build -t my-app .
docker tag my-app:latest <aws-account-id>.dkr.ecr.<region>.amazonaws.com/my-app:latest
Push the image:

docker push <aws-account-id>.dkr.ecr.<region>.amazonaws.com/my-app:latest
6. Setting Up ECS (Using AWS Console)
Step 1: Create an ECS Cluster
Go to AWS Console → ECS → Clusters.
Click Create Cluster.
Choose "Networking only (Fargate)" → Click Next.
Cluster Name: my-cluster.
Click Create.
Step 2: Create a Task Definition
Go to AWS Console → ECS → Task Definitions.
Click Create new Task Definition.
Launch Type: Fargate.
Task Definition Name: my-task.
Task Role: None (default).
Container Name: my-app.
Image: <aws-account-id>.dkr.ecr.<region>.amazonaws.com/my-app:latest.
Port Mapping: 3000.
Memory & CPU: 512 MB, 256 CPU.
Click Create Task Definition.
Step 3: Create ECS Service with ALB
Go to AWS Console → ECS → Clusters → Select my-cluster.

Click Create Service.

Launch Type: Fargate.

Task Definition: Select my-task.

Number of Tasks: 1 (increase later if needed).

Load Balancer: Application Load Balancer.

Name the ALB: my-alb.

Port: 80 HTTP.

Target Group: Keep default.

Under "Networking", select ECS-SG (ECS Security Group).

Click Create Service.

7. Fixing AWS Glitch with ALB Security Group
AWS Glitch:
By default, the ALB gets assigned the ECS Security Group instead of its own.

Go to AWS Console → EC2 Dashboard → Load Balancers.
Click my-alb → Go to Security Groups → Edit.
Change it to ALB-SG.
Click Save.
8. Finalizing Security Groups
Security Group	Inbound Rules	Outbound Rules
ALB-SG	HTTP, Port 80, Source: 0.0.0.0/0	All Traffic 0.0.0.0/0
ECS-SG	TCP, Port 3000, Source: ALB-SG	All Traffic 0.0.0.0/0
9. Testing the Deployment
Find the ALB DNS Name:
aws elbv2 describe-load-balancers --query "LoadBalancers[*].DNSName"
Copy the DNS Name and open in a browser:
http://your-alb-dns-name
🎉 Your app is now live!
10. Cleanup (Optional)
To delete resources:

aws ecs delete-service --cluster my-cluster --service my-service --force
aws ecs delete-cluster --cluster my-cluster
aws ecr delete-repository --repository-name my-app --force
aws elbv2 delete-load-balancer --load-balancer-arn <alb-arn>
aws ec2 delete-security-group --group-id <sg-id>
Conclusion
You have successfully deployed a Dockerized application on AWS using ECS, ECR, ALB, and Security Groups. This setup ensures scalability, security, and high availability. 🚀

For improvements:

Enable HTTPS using ACM SSL
Auto Scale ECS Tasks
Set Up CI/CD with GitHub Actions