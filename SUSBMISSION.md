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

### 6. Kubectl
- **kubectl install**
```bash
  # for linux
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
  echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

  # Install kubectl
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  kubectl version --client
```

### 7. Helms chart
- **Hemsc chart**
```bash
  HELM_BUILDKITE_APT_KEY_ID="DDF78C3E6EBB2D2CC223C95C62BA89D07698DBC6"
  sudo apt-get install curl gpg apt-transport-https --yes
  curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey > "${TMPDIR:-/tmp}/helm.gpg"
  # Ensure that the key ID matches to prevent a repository compromise from establishing an attacker controlled key
  if [ "$(gpg --show-keys --with-colons "${TMPDIR:-/tmp}/helm.gpg" | awk -F: '$1 == "fpr" {print $10}' | head -n 1)" !=                 "${HELM_BUILDKITE_APT_KEY_ID}" ]; then echo "ERROR: Unexpected Helm APT key ID: potential key compromise"; exit 1; fi
  cat "${TMPDIR:-/tmp}/helm.gpg" | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
  echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" |    sudo     tee /etc/apt/sources.list.d/helm-stable-debian.list
  sudo apt-get update
  sudo apt-get install helm
```

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
