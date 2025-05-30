# Java, Jenkins, Docker, Docker Compose and Docker Hub Setup on AWS EC2

This guide outlines the steps to install and configure Java, Jenkins, Docker, Docker Compose and Docker Hub on an AWS EC2 instance running Ubuntu. 

## Prerequisites

- An AWS EC2 instance with Ubuntu.
- A user with `sudo` privileges.

## Steps

### 1. Update the System

Start by updating your system to ensure all packages are up-to-date.

```bash
sudo apt update
```

### 2. Install Java

Jenkins requires Java, so we will install OpenJDK 17.

```bash
sudo apt install openjdk-17-jdk -y
```

Verify the Java installation:

```bash
java -version
```

Ensure it shows something like this:

```
openjdk version "17.0.x" 2021-09-14
OpenJDK Runtime Environment (build 17.0.x+xx)
OpenJDK 64-Bit Server VM (build 17.0.x+xx, mixed mode, sharing)
```

### 3. Add Jenkins GPG Key

Next, you need to add Jenkins' official GPG key to your system for package verification.

```bash
wget -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

### 4. Add Jenkins Repository

Add the Jenkins repository to your sources list.

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### 5. Install Jenkins

Update your package lists and install Jenkins:

```bash
sudo apt update
sudo apt-get install jenkins -y
```

### 6. Start Jenkins

Enable Jenkins to start automatically on boot, and then start the service:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Check the Jenkins service status:

```bash
sudo systemctl status jenkins
```

If the service is running, it should show the status as `active (running)`.

### 7. Install Docker

Installing Docker

```bash
sudo apt install docker.io -y
```

Checking Docker Version

```bash
docker version
```

Giving Permission

```bash
sudo usermod -aG docker $USER && newgrp docker
```

Giving Permission

```bash
sudo usermod -aG docker jenkins
```

Restarting Jenkins

```bash
sudo systemctl restart jenkins
```

Rebooting Instance

```bash
sudo reboot
```


### 8. Connect to Docker Hub

First you need to create an account on `https://hub.docker.com/`.

Login to Docker Hub
```bash
docker login
```

### 9. Install Docker Compose

```bash
sudo apt-get install docker-compose -y
```

### 10. Configure AWS Security Group

Ensure that you can access Jenkins by modifying the inbound rules of your EC2 instance's security group.

- Add a custom TCP rule for port `8080`.
- Set the source to `Anywhere-IPv4` to allow access from anywhere.

### 11. Access Jenkins Web Interface

Open your browser and navigate to the following URL, replacing `<PublicIPv4>` with your EC2 instance's public IPv4 address:

```
http://<PublicIPv4>:8080
```

### 12. Retrieve Jenkins Admin Password

The first time you access Jenkins, it will ask for an initial admin password. Retrieve the password by running this command on your EC2 instance:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it into the login form on the Jenkins web page.

### 13. Set Up Admin Username and Password

After the plugin installation is complete, you’ll be asked to create an admin user.

- **Username**: Abhishek-2502
- Choose your preferred password.

### 14. Install Suggested Plugins

Once logged in, Jenkins will prompt you to install the suggested plugins. Click on "Install suggested plugins" to continue.


### 15. Jenkins is Ready

Once the setup is complete, Jenkins will be ready to use!

---

## Note:

- Always ensure your AWS EC2 instance is properly secured with the necessary firewall rules and access controls.
- The EC2 instance should be monitored for security and performance, especially when hosting Jenkins for production workloads.
