# SRE — Deploy a Python Application on AWS 1-Tier Architecture


# Project Overview

## Introduction

This project demonstrates the deployment of a production-grade python web application using AWS's architecture. 
The implementation follows cloud-native best practices, ensuring high availability, scalability, and security across all application 
tiers.

### Key Features

- **High Availability**: Multi-AZ deployment with automated failover
- **Auto Scaling**: Dynamic resource allocation based on demand
- **Cost Optimization**: Efficient resource utilization and management

### Infrastructure Components

1. **Presentation Tier (Frontend)**
  - Nginx web servers in Auto Scaling Group
  - Public-facing Network Load Balancer
  

2. **Application Tier (Backend)**
  - Apache Tomcat servers in Auto Scaling Group
  - Internal Network Load Balancer
  - Session management with Amazon ElastiCache

3. **Data Tier**
  - Amazon DynamoDB
  - Amazon S3 bucket
  - Automated backups and point-in-time recovery
  - 

### Network Architectur

- **VPC Design**
  - Two separate VPCs (197.126.0.0/16 and subnet 197.126.10.0/24)
  - Public and private subnets across multiple AZs
  - Transit Gateway for inter-VPC communication

# Pre-Requisites

## Required Accounts and Tools

### 1. AWS Account Setup
- Create an [AWS Free Tier Account](https://aws.amazon.com/free/)
- Install AWS CLI v2
```bash
  # For Linux
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip
  sudo ./aws/install

  # For macOS
  brew install awscli

  # Configure AWS CLI
  aws configure
```

### 2. Development Tools
- **Git**: Version control system
```bash
  # For Linux
  sudo apt-get update
  sudo apt-get install git

  # For macOS
  brew install git
```


### 3. CI/CD Integration
- **Github actions**
  - S
  - Generate authentication token
  - Configure project settings:
```bash

### 5. Sonarqueb
- **sonarqueb** for SAST DAST IAT
```bash
  # for linux
  sudo apt install -y unzip wget git
  sudo apt update
  sudo apt install -y openjdk-17-jdk

  sudo wget -O sonarqube.zip "https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.8.100196.zip"
  sudo unzip sonarqube.zip -d /opt/
  sudo mv /opt/sonarqube-9.9.8.100196 /opt/SonarQube
  ls /opt/SonarQube
  sudo mv /opt/sonarqube-9.9.8.100196 /opt/SonarQube

### create a user for sonar queb
  sudo useradd sonar
  sudo chown -R sonar:sonar /opt/SonarQube
  sudo chmod -R 775 /opt/SonarQube

### Start the sonarqueb
  sudo su - sonar -s /bin/bash
  cd /opt/sonarqube/bin/linux-x86-64/
  ./sonar.sh start

### Configure as a System Service (Optional) 
  sudo ln -s /opt/sonarqube/bin/linux-x86-64/sonar.sh /etc/init.d/sonar
  sudo nano /etc/systemd/system/sonar.service
  sudo systemctl daemon-reload
  sudo systemctl enable sonar
  sudo systemctl start sonar
  sudo systemctl status sonar
  sudo rm /etc/init.d/sonar
  sudo journalctl -xeu sonar.service
  cat /opt/sonarqube/logs/sonar.log
  cat /opt/sonarqube/logs/es.log
  sudo su - sonar -s /bin/bash -c "/opt/sonarqube/bin/linux-x86-64/sonar.sh stop"

```

<img width="854" height="454" alt="sonarquebctl" src="https://github.com/user-attachments/assets/2f837d62-97da-4bcb-b0d4-e27d6ac357c6" />

<img width="857" height="430" alt="sonarqube installation" src="https://github.com/user-attachments/assets/146b8273-7d2c-4109-847d-b2412a281f62" />



### 5. install java
- **JAVA**
```bash
  # for linux
  sudo apt install -y unzip wget git
  sudo apt update
  sudo apt install -y openjdk-17-jdk
  
```

### 7. JENKINGS
- **JENKINS**
```bash
  # for linus
  sudo apt update
  sudo apt install fontconfig openjdk-21-jre
  java -version

  sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
  echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt update
  sudo apt install jenkins

  sudo systemctl enable jenkins
  sudo systemctl enable jenkins
  sudo systemctl status jenkins
```

<img width="870" height="431" alt="jenkinssysctl" src="https://github.com/user-attachments/assets/d503b16d-c00e-407e-b40a-8b77d0a25149" />

<img width="847" height="410" alt="jenkins-installed" src="https://github.com/user-attachments/assets/e541bc69-4c70-4450-80f6-920d01cc2d1d" />



### 8. Terraform
- **Terraform**
```bash
  # for linux
  sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
  wget -O- https://apt.releases.hashicorp.com/gpg | \
  gpg --dearmor | \
  sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null


  gpg --no-default-keyring \
  --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
  --fingerprint

  /usr/share/keyrings/hashicorp-archive-keyring.gpg
  -------------------------------------------------
  pub   rsa4096 XXXX-XX-XX [SC]
  AAAA AAAA AAAA AAAA
  uid         [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
  sub   rsa4096 XXXX-XX-XX [E]

  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg]
  https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee
  /etc/apt/sources.list.d/hashicorp.list
  sudo apt update
  sudo apt-get install terraform
```

### 8. Terraform
- **Terraform command used**
```bash
  Terraform init 
  terraform fmt
  terraform validate
  terraform plan
  terraform apply
```

### 3. NEXUS
- **Nexus for storage**
  - S
  - Generate authentication token
  - install java:
```bash
  # for linux
  sudo apt update
  sudo apt install -y openjdk-8-java -version

  cd /opt
  sudo wget https://download.sonatype.com/nexus/3/nexus-3.70.1-02-java8-unix.tar.gz
  sudo tar -zxvf nexus-3.70.1-02-java8-unix.tar.gz
  sudo mv nexus-3.70.1-02 nexus

  # Create the nexus user and set ownership:
  sudo useradd -r -m -U -d /opt/nexus -s /bin/bash nexus
  sudo chown -R nexus:nexus /opt/nexus
  sudo chown -R nexus:nexus /opt/sonatype-work

  set nexus runner to run as nexus
  sudo nano /opt/nexus/bin/nexus.rc
  run_as_user="nexus"

  # create the sysmtd services file
  sudo nano /etc/systemd/system/nexus.service

  paste this
  [Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target

  # start the systmctl
  sudo systemctl daemon-reload
  sudo systemctl enable nexus
  sudo systemctl start nexus
  sudo systemctl status nexus
```

<img width="934" height="416" alt="image" src="https://github.com/user-attachments/assets/561c1053-0c37-4927-bf42-fa19ff88d079" />

<img width="934" height="416" alt="nexus-status" src="https://github.com/user-attachments/assets/5d09f04a-7bcc-4336-8917-9ae4cb13b26c" />

### 3. Tomcat
- **Tomcat for web proxzy**
  - S
  - Generate authentication token
  - install java:
```bash
  sudo hostnamectl set-hostname app-server
  exec bash

  sudo apt update
  sudo apt install -y openjdk-11-jdk
  java -version

. Install Tomcat 9:
  cd /opt
  sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.65/bin/apache-tomcat-9.0.65.tar.gz
  sudo tar -zxvf apache-tomcat-9.0.65.tar.gz
  sudo mv apache-tomcat-9.0.65 tomcat

  Create tomcat user and set ownership
  sudo useradd -r -m -U -d /opt/tomcat -s /bin/bash tomcat
  sudo chown -R tomcat:tomcat /opt/tomcat

  Enable the Manager app — edit tomcat-users.xml:
  sudo nano /opt/tomcat/conf/tomcat-users.xml
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <user username="admin" password="admin123" roles="manager-gui,manager-script"/>

  Allow remote access to Manager app:
  bashsudo nano /opt/tomcat/webapps/manager/META-INF/context.xml

  Comment out the Valve line:
  xml<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
  Create systemd service:
  bashsudo nano /etc/systemd/system/tomcat.service
  Paste this:
  ini[Unit]
  Description=Tomcat 9
  After=network.target

  [Service]
  Type=forking
  User=tomcat
  Group=tomcat
  Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
  Environment="CATALINA_HOME=/opt/tomcat"
  ExecStart=/opt/tomcat/bin/startup.sh
  ExecStop=/opt/tomcat/bin/shutdown.sh
  Restart=on-abort

  [Install]
  WantedBy=multi-user.target
  Enable and start Tomcat:
  bashsudo systemctl daemon-reload
  sudo systemctl enable tomcat
  sudo systemctl start tomcat
  sudo systemctl status tomcat

  Install Nginx:
  sudo apt install -y nginx
  Configure Nginx reverse proxy:
  sudo nano /etc/nginx/sites-available/default

  Replace the contents with:
  nginxserver {
      listen 80;
      server_name _;

      location / {
          proxy_pass http://localhost:8080;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
        }
  }
  Start Nginx:
  sudo systemctl enable nginx
  sudo systemctl start nginx
  sudo systemctl status nginx

```

<img width="874" height="461" alt="tomcat-installed" src="https://github.com/user-attachments/assets/e007cd42-87ff-4e77-88ac-f586d2db23cd" />

<img width="886" height="451" alt="image" src="https://github.com/user-attachments/assets/435a3239-3a7e-4be1-8530-85acae1884f2" />






