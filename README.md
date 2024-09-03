# Web Application on AWS using an Auto Scaling Group

This guide demonstrates how to deploy a web application on AWS using an Auto Scaling Group. It covers the prerequisites, overview of the deployment, and step-by-step instructions to set up the infrastructure.

## Overview

Auto Scaling Groups (ASGs) automatically adjust the number of EC2 instances running your application based on demand. This helps ensure that your application remains highly available and can handle varying traffic loads.

In this guide, we'll deploy a simple web application on AWS EC2 instances, configure a Load Balancer, and set up an Auto Scaling Group to automatically scale the number of instances based on traffic.

## Prerequisites

1. **AWS Account**: You need an AWS account to set up resources.
2. **AWS CLI**: Ensure you have the AWS CLI installed and configured. Follow the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) if needed.
3. **IAM Permissions**: Ensure your IAM user has permissions to create EC2 instances, Launch Configurations, Auto Scaling Groups, and Load Balancers.

## Requirements

- **Amazon EC2**: To run your web application.
- **Amazon S3**: To store static files (optional).
- **Amazon RDS**: For database (optional, depending on your web application).
- **Amazon Elastic Load Balancer (ELB)**: To distribute incoming traffic across EC2 instances.
- **Amazon Auto Scaling**: To automatically scale the number of EC2 instances based on traffic.

## Step-by-Step Instructions

### 1. Create an EC2 Launch Template

1. **Sign in to AWS Management Console**:
   - Navigate to the EC2 Dashboard.

2. **Create a Launch Template**:
   - Go to "Launch Templates" under "Instances".
   - Click "Create launch template".
   - Provide a name (e.g., `my-web-app-template`).
   - Choose an Amazon Machine Image (AMI) (e.g., Amazon Linux 2).
   - Select an instance type (e.g., `t2.micro`).
   - Configure security groups to allow HTTP (port 80) and SSH (port 22) access.
   - Specify a key pair for SSH access.
   - Click "Create launch template".

### 2. Create an Elastic Load Balancer (ELB)

1. **Navigate to the Load Balancers**:
   - Go to the EC2 Dashboard and select "Load Balancers".

2. **Create a Load Balancer**:
   - Click "Create Load Balancer".
   - Choose "Application Load Balancer" or "Network Load Balancer" based on your needs.
   - Configure the load balancer with a name (e.g., `my-web-app-load-balancer`), and select a VPC and subnets.
   - Configure listeners (e.g., HTTP on port 80).
   - Set up a security group that allows HTTP traffic.
   - Configure health checks to monitor the health of your EC2 instances.
   - Click "Create".

### 3. Create an Auto Scaling Group

1. **Navigate to the Auto Scaling Groups**:
   - Go to the EC2 Dashboard and select "Auto Scaling Groups".

2. **Create an Auto Scaling Group**:
   - Click "Create Auto Scaling group".
   - Choose "Launch Template" and select the launch template created earlier.
   - Configure the group with a name (e.g., `my-web-app-auto-scaling`).
   - Choose the VPC and subnets.
   - Attach the previously created Load Balancer.
   - Define the desired, minimum, and maximum number of instances.
   - Configure scaling policies (e.g., scale up or down based on CPU utilization).
   - Review and create the Auto Scaling Group.

### 4. Deploy the Web Application

1. **Connect to an EC2 Instance**:
   - Use SSH to connect to one of your EC2 instances and deploy your web application. Example command:
     ```bash
     ssh -i "your-key.pem" ec2-user@your-instance-public-dns
     ```

2. **Install Web Server and Application**:
   - Install necessary packages (e.g., Apache, Node.js) and deploy your web application.

   **Example commands for a simple Node.js app**:
   
   ```bash
   sudo yum update -y
   sudo yum install -y nodejs npm
   mkdir my-app
   cd my-app
   npm init -y
   npm install express
```

	**Example `index.js`**:

```Node
const express = require('express');
const app = express();
const port = 80;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});

```

	**Start the Node.js app**:

```node
node index.js
```

3. **Verify Deployment**:
	- Access the Load Balancer DNS name in your browser to verify that the web application is up and running.

## Conclusion

You have successfully set up an Auto Scaling Group to deploy and manage a web application on AWS. The Auto Scaling Group will automatically adjust the number of EC2 instances based on the traffic load, ensuring high availability and scalability for your application.

## Additional Resources

- [AWS Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Elastic Load Balancing Documentation](https://docs.aws.amazon.com/elasticloadbalancing/)
- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/latest/userguide/)

