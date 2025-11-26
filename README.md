# Cloudformation-three-tier-architecture
This is a clouformation template that provides Infrastructure as Code (IaC). The template creates an Application Load Balancer, Auto Scaling Group of EC2 instance and an RDS Database

# Description
This repository contains a CloudFormation template that provisions a highly available three-tier architecture on AWS across public and private subnets.

- **Tier 1 (Public):** Internet-facing Application Load Balancer (ALB) in two public subnets.
- **Tier 2 (Private/App):** Auto Scaling Group (ASG) of EC2 instances in two private subnets. User data installs and starts Nginx and serves a basic page.
- **Tier 3 (Private/Data):** Amazon RDS for MySQL deployed in private subnets via a DB Subnet Group.

The stack builds the networking (VPC, subnets, route tables, IGW, NAT Gateway), security groups for ALB/EC2/RDS, an ALB + listener + target group, a Launch Template + ASG, and an RDS instance.

## Prerequisites
- AWS account and permissions to create networking, compute, and database resources
- Default VPC is not required; this template creates its own VPC
- Ensure you deploy in region `eu-west-2` (London) or update `Mappings/Images` and AZ parameters

## Deployment
You can deploy via AWS Console or AWS CLI.

### AWS Console
1. Open CloudFormation in your target region.
2. Create stack with new resources.
3. Upload the yml file.
4. Provide parameters (override `DBPassword` with a strong secret; choose an allowed `InstanceType`).
5. Acknowledge IAM capabilities if prompted and create the stack.

## Notes & Considerations
- For production, tighten Security Groups (remove 0.0.0.0/0 for SSH, use SSM Session Manager, restrict ALB ports as needed).
- Consider enabling Multi-AZ for RDS and increasing storage as required.
- NAT Gateways incur hourly and data processing costs; ensure you need outbound access from private subnets.
- The AMI mapping currently targets `eu-west-2`. Update `Mappings` if deploying elsewhere.
- The app target group listens on port 3000 for NodeJs app, adjust target port to match your app.
