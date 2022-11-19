# Deployment 5 Documentation: Deploying to a Container Architecture
   By Anjuli Panizzi   November 18th, 2022 
# Purpose of the Deployment:
   The purpose of this deployment is to learn how to use Jenkins, Docker, and Terraform to deploy a containerized Flask application. The Flask application is a url-shortener.
# Steps of the Deployment:
1. Install Jenkins on an EC2 in the default VPC.
   - a. Run sudo apt update.
   - b. Run sudo apt upgrade.
   - c. Run the command sudo apt install to install the following packages: default-jre, python3-
  pip, python3.10-venv and nginx.
2. Install Docker on an EC2 in the default VPC.
    - a. Add the script below to the user data field while launching the EC2 ont the AWS console.

```
 #!/bin/bash
# Install docker
apt-get update
apt-get install -y cloud-utils apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
apt-get update
apt-get install -y docker-ce
usermod -aG docker ubuntu
# Install docker-compose
curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
 ```
 
   -b. Run the hello-world image to verify that the Docker Engine installation is successful.
   
    ```
    $sudo docker run hello-world 
    ```
    
    -c. This command downloads the hello-world image and runs it in a container. 
        When the container successfully runs, a confirmation message is printed and the container exits.
        
3. Install Terraform on an EC2 in the default VPC.
   -a. Installed Terraform on the EC2 using sudo snap install terraform
   ![image](https://user-images.githubusercontent.com/108234350/202850430-822bda79-2beb-4e6a-9866-9f5d0d121b6a.png)

# Configure Jenkins Console
   -a. I connected to the Jenkins webserver using the public ip
and port 8080.
   -b. I installed and configured Terraform on the Jenkins web
server using Go to Manage Jenkins > Manage Plugins >Available >
search Terraform. Configure Terraform
o![image](https://user-images.githubusercontent.com/108234350/202843272-bb2b895f-4567-4fff-b413-d70c544afd3d.png)
  -c. Then I navigated to Go to Manage Jenkins > Global Tool
Configuration > Terraform displayed on the list
![image](https://user-images.githubusercontent.com/108234350/202843323-5be52958-97cb-490e-9214-d285abb360bf.png)
  -d. Next, I configured my AWS credentials on Jenkins.
  ![image](https://user-images.githubusercontent.com/108234350/202849324-bd357202-4a5a-48d0-ae93-e16425511bbb.png)
  -e. Next, I created my Dockerfile.
  -f. Then, I created my Jenkinsfile.
  -h. I added the Docker Agent to Jenkins
  ![image](https://user-images.githubusercontent.com/108234350/202851015-2c87e494-6375-40fc-ae77-3d15a61ff44f.png)
  -i. I added the Terraform Agent to Jenkins
  -g. I then connected my Github credentials to the
Jenkins server using the personal access token that I generated on Github.
Unfortuately the build failed and the application did not deploy.
![image](https://user-images.githubusercontent.com/108234350/202856603-aa906b68-6613-4079-b8aa-987f34ce8407.png)


#Issues
1. I was unable to install Docker Engine from the Docker apt repository. Therefore I attempted to install Docker engine from the .deb files. This attempt also failed. So I terminated that Docker EC2.
As a solution, I launched a new EC2 from the AWS installed Docker and Docker Engine by adding a the script to the user data field prior to launch.
2. Build failed.
3. Application did not deploy.
