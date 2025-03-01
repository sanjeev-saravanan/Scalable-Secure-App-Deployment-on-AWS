##The architecture consists of:

Web Tier: Public-facing Application Load Balancer routes traffic to Nginx web servers hosting a React.js website.

Application Tier: Internal Load Balancer directs API calls to Node.js application servers.

Database Tier: Aurora MySQL database (multi-AZ) for data storage and manipulation.

Scalability & Availability: Load balancers, health checks, and autoscaling groups are implemented at each tier to ensure high availability and scalability.

Step-by-Step Implementation
Step 1: Download Code from GitHub
Clone the repository to your local system to access the required code and configuration files.

Step 2: Create S3 Buckets
Code Storage Bucket: Create an S3 bucket to store web-server and app-server code. Upload the code from your local system.

VPC Flow Logs Bucket: Create a second S3 bucket to store VPC flow logs.

Step 3: Create IAM Role
Create an IAM role with the following policies:

AmazonS3ReadOnlyAccess: For accessing code from S3.

AmazonSSMManagedInstanceCore: For enabling SSM access to EC2 instances.

Step 4: Set Up VPC and Networking
Create a VPC with public and private subnets.

Enable auto-assign public IP for web-tier public subnets.

Create and attach an Internet Gateway (IGW) and NAT Gateway (NAT-GW).

Configure route tables (RT) for public and private subnets.

Enable VPC flow logs and store them in the S3 bucket created earlier.

Step 5: Create Security Groups
External-Load-Balancer-SG: Allow HTTP (port 80) from 0.0.0.0/0.

Web-Tier-SG: Allow HTTP traffic from the External Load Balancer SG.

Internal-Load-Balancer-SG: Allow HTTP traffic from the Web-Tier SG.

App-Tier-SG: Allow traffic on port 4000 from the Internal Load Balancer SG.

DB-Tier-SG: Allow MySQL (port 3306) traffic from the App-Tier SG.

Step 6: Set Up Database Tier
Create a DB subnet group.

Deploy an Aurora MySQL database (multi-AZ) within the DB subnet group.

Step 7: Configure Application Tier
Launch a test app server and install required packages.

Test connections to the database and other components.

Create an AMI of the configured app server.

Use the AMI to create a launch template.

Set up a target group and internal load balancer.

Configure an autoscaling group for the application tier.

Update the nginx.conf file with the Internal Load Balancer DNS and upload it to S3.

Step 8: Configure Web Tier
Launch a test web server and install Nginx, Node.js, and React.js.

Test connections to the application tier.

Create an AMI of the configured web server.

Use the AMI to create a launch template.

Set up a target group and external load balancer.

Configure an autoscaling group for the web tier.

Step 9: Configure DNS with Route 53
Add a DNS record in Route 53 pointing to the External Load Balancer DNS.

Step 10: Set Up Monitoring and Alerts
Create CloudWatch alarms for key metrics (e.g., CPU utilization, latency).

Configure SNS topics for notifications.

Step 11: Enable Logging with CloudTrail
Enable AWS CloudTrail to log API activity for auditing and troubleshooting.

Step 12: Clean Up Resources
Delete all resources in the following order:

CloudFront distributions.

CloudWatch alarms and SNS topics.

Route 53 DNS records.

Load balancers, target groups, autoscaling groups, and launch templates.

Security groups.

NAT Gateway and release associated Elastic IP.

VPC.

RDS instance and DB subnet group.

References
App-Server Commands

Web-Server Commands
