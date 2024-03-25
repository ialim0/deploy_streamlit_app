
# Deploying Streamlit App on AWS

This guide outlines the process for deploying a Streamlit application on AWS, including setting up the necessary AWS infrastructure, deploying the Neo4j database, and ensuring the application is accessible.

## Prerequisites

- AWS account with necessary permissions.
- AWS CLI installed and configured on your local machine.
- Basic knowledge of AWS services (EC2, S3, IAM, Route 53).
- A Streamlit application ready for deployment.

## Steps for Deployment

### Step 1: Clone the GitHub Repository

```bash
git clone https://github.com/neo4j-partners/hands-on-lab-neo4j-and-bedrock.git
cd hands-on-lab-neo4j-and-bedrock
```

### Step 2: Deploy Neo4j on AWS

- Detailed instructions are provided in the "Lab 1 - Deploy Neo4j" section within the GitHub repository.

### Step 3: Set Up EC2 Instance

1. **Create an EC2 Instance**: Navigate to EC2 in the AWS Management Console, and launch a new instance using an Amazon Linux 2 AMI.
2. **Configure Security Group**: Modify the security group to allow inbound traffic on port 8501, the default port for Streamlit apps.

### Step 4: Install Streamlit and Dependencies

SSH into your EC2 instance and run:

```bash
sudo yum update -y
pip install streamlit
# Add other necessary dependencies here
```

### Step 5: Upload Your App

Upload your Streamlit app to the EC2 instance:

```bash
scp -i /path/to/your/key.pem /path/to/your/app/folder ec2-user@your-ec2-instance-ip:/home/ec2-user/
```

### Step 6: Run Your Streamlit App

Start your app on the EC2 instance:

```bash
streamlit run /home/ec2-user/your-app-folder/your_app.py --server.port 8501
```

### Step 7: Set Up Nginx Reverse Proxy (Optional)

Install Nginx and configure it as a reverse proxy to forward traffic from port 80 to your Streamlit app on port 8501:

```bash
sudo yum install nginx
# Edit Nginx configuration as shown in the guide
sudo systemctl start nginx.service
```

### Step 8: Configure Hosted Zone on Route 53 (Optional)

- Register a domain with Route 53 and configure the hosted zone.
- Add an A record pointing to the public IP address of your EC2 instance.

### Step 9: Test Your App

Access your Streamlit app via `http://your-ec2-instance-ip:8501` or your custom domain name, if configured.

## Troubleshooting

- Confirm security group settings.
- Review Nginx and Route 53 configurations for errors.

## Conclusion

You should now have a fully functional Streamlit application running on AWS, accessible through a public IP or a custom domain. For further assistance, contact AWS support or refer to AWS documentation.

---

## Additional Notes

- The steps above assume a Linux-based environment on both the user's local machine and the AWS EC2 instance.
- The specific commands for installing dependencies may vary based on the requirements of the Streamlit application.
- The Nginx and Route 53 configuration steps are optional and depend on whether a custom domain is used.

Remember to replace placeholder text like `/path/to/your/`, `your-ec2-instance-ip`, `your-app-folder`, and `your_app.py` with actual paths, IP addresses, folder names, and filenames relevant to your deployment.

