# -Building-a-basic-Docker-image-and-pushing-it-to-AWS-Elastic-Container-Registry-ECR-
Task 1: Install Docker
You can install Docker by following the official instructions for your operating system. Here's a general guideline:

For Ubuntu:
sudo apt-get update
sudo apt-get install docker.io


For CentOS:
sudo yum install docker
sudo systemctl start docker
sudo systemctl enable docker
For macOS or Windows, download and install Docker Desktop from the official Docker website.
Task 2: Install AWS CLI
You can install the AWS Command Line Interface (CLI) using Python's pip package manager. Make sure you have Python installed. Then run:
pip install awscli

After installation, you may need to configure the CLI by running aws configure and providing your AWS access key, secret key, region, and default output format.

Task 3: Create Dockerfile
Create a Dockerfile in your project directory. Here's a basic example:

Dockerfile

# Use an light weight runtime as the base image
FROM nginx : alpine

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

Task 4: Build the Docker image
Navigate to the directory containing your Dockerfile and run:


docker build -t my-image .

Task 5: Authenticate AWS ECR
Authenticate Docker to an Amazon ECR registry using the AWS CLI:

aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
Replace <region> with your AWS region and <account-id> with your AWS account ID.

Task 6: Create an ECR repository
Create a repository in Amazon ECR using the AWS CLI:
aws ecr create-repository --repository-name my-repo --region <region>


Task 7: Tag the Docker image
Tag your Docker image with the ECR repository URI:

docker tag my-image:latest <account-id>.dkr.ecr.<region>.amazonaws.com/my-repo:latest
Task 8: Push the Docker image to ECR
Push the Docker image to your Amazon ECR repository:


docker push <account-id>.dkr.ecr.<region>.amazonaws.com/my-repo:latest
Task 9: Verify the image in ECR
Verify that the image has been pushed successfully to Amazon ECR by checking the repository in the AWS Management Console or by using the AWS CLI:


aws ecr describe-images --repository-name my-repo --region <region>
