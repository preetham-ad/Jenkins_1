# Jenkins_1
setting up Jenkins on an AWS EC2 instance and configuring it to work with Docker
# Jenkins Setup on AWS EC2 with Docker Integration

This guide explains how to set up Jenkins on an AWS EC2 instance and configure it to work with Docker for managing build and deployment tasks.

## Prerequisites
- An AWS account
- Basic knowledge of AWS EC2
- An EC2 instance running Ubuntu

## Steps

### 1. Setting Up AWS EC2 Instance
1. **Go to the AWS Management Console:**
   - Log in to your AWS account and navigate to the EC2 dashboard.
   
2. **Navigate to "Instances" under EC2:**
   - This is where you can manage your virtual servers.
   
3. **Click "Launch Instance" to create a new virtual server:**
   - Select an Ubuntu image, choose an instance type, and configure the instance details as needed.

### 2. Install Java
1. **Update the package list:**
    ```bash
    sudo apt update
    ```
   - This ensures your package list is up-to-date.

2. **Install Java:**
    ```bash
    sudo apt install openjdk-11-jre
    ```
   - Jenkins requires Java to run. OpenJDK 11 is a stable version that works well with Jenkins.

3. **Verify the installation:**
    ```bash
    java -version
    ```
   - This command checks if Java is properly installed by displaying the version.

### 3. Install Jenkins
1. **Add the Jenkins package repository:**
    ```bash
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    ```
   - This adds the Jenkins repository key to your system and includes the Jenkins package in your sources list.

2. **Update the package list:**
    ```bash
    sudo apt-get update
    ```
   - This updates the package list to include the Jenkins packages.

3. **Install Jenkins:**
    ```bash
    sudo apt-get install jenkins
    ```
   - This installs Jenkins using the package manager.

### 4. Configure Security Settings
1. **Go to the EC2 console, select your instance, and navigate to the "Security" tab:**
   - Security settings are crucial to control access to your instance.
   
2. **Click on "Security Groups" and add an inbound rule to allow traffic on port 8080:**
   - Jenkins by default runs on port 8080. This step ensures that you can access Jenkins from your web browser.

### 5. Access Jenkins
1. **Find your EC2 instance's public IP address:**
   - You need this IP address to connect to your Jenkins instance from a browser.

2. **Open `http://<ec2-instance-public-ip>:8080` in your browser:**
   - This URL lets you access the Jenkins web interface.

### 6. Initial Jenkins Setup
1. **Retrieve the Jenkins admin password:**
    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
   - Jenkins generates an initial admin password, which is needed for the first login.

2. **Log in to Jenkins using this password:**
   - Enter the password retrieved in the previous step.

3. **Click on "Install suggested plugins" to get essential plugins:**
   - Plugins extend Jenkins' functionality. The suggested plugins include tools needed for basic operations.

### 7. Create Jenkins Admin User
1. **After installing plugins, Jenkins will prompt you to create an admin user:**
   - Creating an admin user gives you control over Jenkins and is better for long-term management.

### 8. Install Docker Pipeline Plugin
1. **Go to "Manage Jenkins" > "Manage Plugins":**
   - This is where you manage Jenkins plugins.
   
2. **Search for "Docker Pipeline" in the Available tab and install it:**
   - The Docker Pipeline plugin allows Jenkins to work with Docker, enabling it to build and deploy applications using Docker containers.
   
3. **Restart Jenkins:**
   - Restart Jenkins to apply the new plugin.

### 9. Install Docker on EC2 Instance
1. **Update the package list:**
    ```bash
    sudo apt update
    ```
   - Ensure your package list is up-to-date.

2. **Install Docker:**
    ```bash
    sudo apt install docker.io
    ```
   - Docker is needed for running containers. This step installs Docker on your EC2 instance.

3. **Grant permissions to Jenkins and the Ubuntu user:**
    ```bash
    sudo usermod -aG docker jenkins
    sudo usermod -aG docker ubuntu
    sudo systemctl restart docker
    ```
   - These commands give Jenkins and the Ubuntu user permission to interact with Docker, allowing Jenkins to use Docker containers in its builds.

### 10. Restart Jenkins
1. **Restart Jenkins from the URL: `http://<ec2-instance-public-ip>:8080/restart`:**
   - Restarting Jenkins ensures that all configurations and plugin installations are properly applied.

## Summary
- Set up a virtual server on AWS.
- Install Java (Jenkins needs it to run).
- Install Jenkins on this server.
- Adjust security settings to allow access to Jenkins.
- Access Jenkins through your web browser to finish setup.
- Install the Docker plugin to let Jenkins use Docker.
- Install Docker on your server and configure Jenkins to use Docker.

This setup enables you to use Jenkins for automating your software builds, tests, and deployments using Docker containers.

