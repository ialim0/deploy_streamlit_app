
# Deploying Streamlit App on AWS

This guide provides a comprehensive step-by-step process to deploy a Streamlit application on AWS, including setting up the necessary infrastructure, deploying the application, and configuring access.

## Prerequisites

- Basic understanding of AWS services, EC2, security groups, and networking.
- A fully developed Streamlit application.
- AWS account with necessary permissions.
- AWS CLI installed and configured on your local machine.

## Step 1: Clone the GitHub Repository

Clone the GitHub repository containing your Streamlit application to your local machine:

```bash
git clone https://github.com/neo4j-partners/hands-on-lab-neo4j-and-bedrock.git
cd hands-on-lab-neo4j-and-bedrock



## Step 2: Deploy Neo4j on AWS

Follow the instructions provided in the GitHub repository under "Lab 1 - Deploy Neo4j" to deploy Neo4j on AWS.

## Step 3: Setup EC2 Instance

1. **Create an EC2 Instance**: In the AWS Management Console, navigate to EC2 and create a new instance. Choose an Amazon Linux 2 AMI.
2. **Configure Security Group**: Ensure the security group associated with your EC2 instance allows inbound traffic on port 8501 (default port for Streamlit apps).

## Step 4: Install Streamlit and Dependencies

SSH into your EC2 instance and install Streamlit and any other dependencies required by your app:

```bash
pip install streamlit

# Install other dependencies as needed

## Step 5: Upload Your App

Use SCP or any other method to upload your Streamlit app files to the EC2 instance:

```bash
scp -i /path/to/your/key.pem /path/to/your/app/folder ec2-user@your-ec2-instance-ip:/home/ec2-user/
## Step 6: Run Your Streamlit App

Navigate to your app's directory on the EC2 instance and start the Streamlit app:

```bash
cd /home/ec2-user/your-app-folder 
streamlit run your_app.py

## Step 7: Configure Nginx Reverse Proxy (Optional)

If you want to use a custom domain name and route traffic from port 80 to your Streamlit app running on port 8501, configure an Nginx reverse proxy:

1. Install Nginx on your EC2 instance:

```bash
sudo yum install nginx
2. Create a new Nginx configuration file in `/etc/nginx/conf.d/` with the following content, replacing `<Private IP>` with your EC2 instance's private IP address:

```nginx
upstream ws-backend {
    server <Private IP>:8501;
}

server {
    listen 80;
    server_name <Private IP>;
}

location / {
    proxy_pass http://ws-backend;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}


3. Start Nginx:

```bash
sudo systemctl start nginx.service


## Step 8: Configure Hosted Zone on Route 53 (Optional)

If you have a custom domain name, configure a hosted zone in Amazon Route 53 to enable a custom DNS name for your application:

1. Register a custom domain name with Route 53.
2. Configure a hosted zone for your domain, entering a record name for your domain prefix and the IP address of your Elastic IP in the value section.

## Step 9: Test Your App

After deploying your Streamlit app, test it by accessing the public IP address of your EC2 instance on port 8501 (e.g., `http://your-ec2-instance-ip:8501`). If you configured a custom domain name, use that instead.

## Troubleshooting

- Ensure your EC2 instance's security group allows inbound traffic on port 8501.
- Check the Nginx configuration for any errors.
- Verify that your custom domain name is correctly configured in Route 53.

## Conclusion

## Thanks Nurman !


 .


