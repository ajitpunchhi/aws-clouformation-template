# AWS CloudFormation Templates Repository

This repository contains a collection of AWS CloudFormation templates for provisioning and managing various AWS resources. These templates are designed to help you quickly set up infrastructure components with proper configurations and security best practices.

## Repository Structure

```
aws-cloudformation-templates/
├── EC2/
│   ├── aws-ec2-cloudformation-template.yaml
│   └── aws-elb-ec2-cloudformation-template.yaml
├── S3/
│   └── aws-s3-cloudformation-template.yaml
├── VPC/
│   └── aws-vpc-cloudformation-template.yaml
└── README.md
```

## Templates Overview

### EC2 Templates

#### Basic EC2 Instance (`EC2/aws-ec2-cloudformation-template.yaml`)

Sets up a basic EC2 instance with the following features:
- Security group with SSH access (port 22)
- Uses Amazon Linux 2 AMI on t2.micro instance type (Free tier eligible)
- Termination protection enabled
- EBS root volume configured not to delete on termination
- Basic user data script to install and start Apache web server

**Requirements:**
- An existing EC2 Key Pair for SSH access
- Default VPC and subnet IDs

#### ELB with EC2 Instances (`EC2/aws-elb-ec2-cloudformation-template.yaml`)

Creates a complete application stack with:
- Custom VPC with public subnets in two availability zones
- Internet Gateway and proper route tables
- Two EC2 instances deployed across different availability zones
- Application Load Balancer (ALB) for distributing traffic
- Security group allowing HTTP (port 80) and SSH (port 22) access
- EC2 instances configured with termination protection and volume retention
- User data scripts to install and configure Apache web server with different content

**Requirements:**
- An existing EC2 Key Pair for SSH access

### S3 Template (`S3/aws-s3-cloudformation-template.yaml`)

Creates an S3 bucket with comprehensive security and lifecycle configurations:
- Versioning enabled to track changes and prevent accidental deletions
- Server-side encryption with AES-256
- Public access blocking for enhanced security
- HTTPS enforcement via bucket policy
- Lifecycle policies for cost optimization:
  - Transition current objects to Standard-IA after 30 days
  - Transition current objects to Glacier after 90 days
  - Move non-current versions to Glacier after 30 days
  - Automatic deletion of current objects after 365 days
  - Automatic deletion of non-current versions after 365 days

### VPC Template (`VPC/aws-vpc-cloudformation-template.yaml`)

Creates a three-tier network architecture suitable for production applications:
- VPC with DNS support and hostnames enabled
- Public tier with internet access via Internet Gateway (2 AZs)
- Private application tier with outbound internet access via NAT Gateway (2 AZs)
- Isolated database tier with no internet access (2 AZs)
- Proper route tables and subnet associations

## How to Use These Templates

### Prerequisites
- AWS CLI installed and configured with appropriate permissions
- Basic understanding of AWS CloudFormation and the target services

### Deployment Instructions

1. **Validate a template**
   ```bash
   aws cloudformation validate-template --template-body file://path/to/template.yaml
   ```

2. **Create a stack**
   ```bash
   aws cloudformation create-stack \
     --stack-name MyStack \
     --template-body file://path/to/template.yaml \
     --parameters ParameterKey=KeyName,ParameterValue=my-key-pair
   ```

3. **Update an existing stack**
   ```bash
   aws cloudformation update-stack \
     --stack-name MyStack \
     --template-body file://path/to/template.yaml \
     --parameters ParameterKey=KeyName,ParameterValue=my-key-pair
   ```

4. **Delete a stack**
   ```bash
   aws cloudformation delete-stack --stack-name MyStack
   ```

## Security Considerations

These templates implement several security best practices:
- EC2 instances have termination protection enabled
- S3 buckets block public access by default
- S3 bucket policies enforce HTTPS
- VPC architecture isolates database resources
- Security groups limit access to necessary ports only

However, you should review and adjust the following:
- The EC2 templates allow SSH access from any IP (`0.0.0.0/0`). Consider restricting this to your specific IP range.
- Update the AMI IDs to the latest secure versions for your region
- Consider adding additional security controls based on your organization's requirements

## Template Parameters

### EC2 Basic Template
- `KeyName`: Name of an existing EC2 KeyPair
- `DefaultVPC`: ID of the default VPC
- `DefaultSubnet`: ID of the default subnet

### ELB with EC2 Template
- `KeyName`: Name of an existing EC2 KeyPair

### S3 Template
- No parameters required (bucket name is derived from stack name)

### VPC Template
- No parameters required (uses default AZs)

## License

[Insert your license information here]

## Contributing

[Insert contribution guidelines here]
