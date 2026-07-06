# Jenkins — Deploy a Java Application on AWS 1-Tier Architecture


# Project Overview

## Introduction

This project demonstrates the deployment of a production-grade java web application using AWS's architecture. 
The implementation follows cloud-native best practices, ensuring high availability, scalability, and security across all application 
tiers.

## Artitecture of the project

<img width="709" height="394" alt="Screenshot 2026-07-06 022539" src="https://github.com/user-attachments/assets/084ac2ff-f190-464d-9960-abd89aea988e" />


### Key Features

- **High Availability**: Multi-AZ deployment with automated failover
- **Auto Scaling**: Dynamic resource allocation based on demand
- **Cost Optimization**: Efficient resource utilization and management

### Infrastructure Components

1. **Presentation Tier (Frontend)**
  - Nginx web servers in Auto Scaling Group
  - Public-facing Network Load Balancer


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

### 4. Sonarqueb
- **sonarqueb for SAST DAST IAT**
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

### 6. JENKINGS
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



### 7. Terraform
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

### 9. NEXUS
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

### 10. Tomcat
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


# Jenkins configurations step
- sudo cat /var/lib/jenkins/secrets/initialAdminPassword
- Paste that password into the Jenkins UI, then:
- Click Install suggested plugins and wait
- Create your admin user
- Click Save and Finish

  <img width="959" height="453" alt="create a jenkins plugins" src="https://github.com/user-attachments/assets/8fa3ea26-a22e-49c6-b2a6-e640eb1debe3" />

  <img width="959" height="430" alt="jenkins-plugins-installed" src="https://github.com/user-attachments/assets/a18c288c-6824-4db7-aaf8-c073fa58a2de" />



## step 2
- Manage Jenkins → Plugins → Available plugins
- Search and install each of these:
- Maven Integration
- SonarQube Scanner
- Nexus Artifact Uploader
- Deploy to container
- GitHub Integration
- Slack Notification (bonus)
- Email Extension (bonus

<img width="950" height="439" alt="jenkins-dashboard" src="https://github.com/user-attachments/assets/5209922b-b694-4357-b728-d7d3d753cdec" />

<img width="953" height="463" alt="plugins-installed" src="https://github.com/user-attachments/assets/921c1bff-79b8-4325-8d20-9eb600bae49d" />


### steps 3
-  Go to Manage Jenkins → Tools
- JDK section:

- Click Add JDK
- Name: JDK11
- Uncheck Install automatically
- JAVA_HOME: /usr/lib/jvm/java-11-openjdk-amd64

- Click Add Maven
- Name: Maven3
- Check Install automatically
- Select version 3.9.6

# Go to Manage Jenkins → Credentials → System → Global credentials (unrestricted) → Add Credentials

- Kind: Username with password
- Username: Davix4u
- Password: paste your GitHub Personal Access Token (PAT)
- ID: github-credentials
- Description: GitHub PAT
- Click Create

## To generate a PAT on GitHub:

- Go to github.com → click your profile → Settings
- Scroll down to Developer settings
- Personal access tokens → Tokens (classic)
- Click Generate new token (classic)
- Give it a name, set expiration, and check these scopes:

repo
admin:repo_hook

<img width="898" height="242" alt="Screenshot 2026-07-03 082235" src="https://github.com/user-attachments/assets/48611ebf-6101-4a9f-9b7c-6029f84b5a09" />

## Go to Jenkins dashboard → New Item (or Create Job)

- Name: Java-Login-App
- Select Freestyle project
- Click OK

- <img width="932" height="422" alt="image" src="https://github.com/user-attachments/assets/aa56b18c-cc1f-42e0-bc84-0d91149ab71a" />

- <img width="957" height="428" alt="jenkins-github" src="https://github.com/user-attachments/assets/8718cc87-94c6-47a9-ab0c-f016d08705df" />

- <img width="949" height="376" alt="jenkins-github-trigger" src="https://github.com/user-attachments/assets/92f6e280-0600-4143-8278-8ed7600acf0f" />

- <img width="959" height="427" alt="github-maven" src="https://github.com/user-attachments/assets/974bf2fb-5f91-4199-9cd6-eb81a905cbfd" />

- <img width="932" height="422" alt="jenkins post job" src="https://github.com/user-attachments/assets/44b78b6b-540b-4b06-9e57-e476bd980894" />
- <img width="932" height="422" alt="Screenshot 2026-07-03 093121" src="https://github.com/user-attachments/assets/e596173d-6b1a-44df-8231-d167089d8ff1" />

## webhook activation
 Click Settings → Webhooks → Add webhook
- Payload URL: http://54.77.44.206:8080/github-webhook/
- Content type: application/json
- Which events: Just the push event
- Check Active
- Click Add webhook

- <img width="952" height="438" alt="webhook-config" src="https://github.com/user-attachments/assets/ea91c7c0-a4a7-44ce-acc7-00fadb298d8e" />

- <img width="905" height="434" alt="webhoo-added" src="https://github.com/user-attachments/assets/94ee410c-ed5d-4dfd-859b-1e0d5ef43756" />
- <img width="955" height="482" alt="tomcat-password" src="https://github.com/user-attachments/assets/ee4d48b4-c38b-4a97-94d9-06ec07023461" />
- <img width="938" height="476" alt="tom-cat=working" src="https://github.com/user-attachments/assets/b5d2e5ad-6759-45f6-a81e-7f22bcdf0e10" />

http://3.250.69.170:8080/Java-login-app-1.0

- <img width="943" height="434" alt="jenkins-deploymentsucessful" src="https://github.com/user-attachments/assets/5ebe0e49-63ae-4406-a360-76b0187675a4" />
- <img width="917" height="455" alt="Screenshot 2026-07-03 112526" src="https://github.com/user-attachments/assets/dcb36098-795d-408b-9d06-b0ba4b63f7d6" />

## Sonaqueb Integration
http://34.245.151.213:9000
Is the SonarQube dashboard accessible? Login with admin / admin if prompted

## In SonarQube dashboard:

- Click your profile icon (top right) → My Account
- Click Security tab
- Under Generate Tokens, enter name: jenkins-token
- Click Generate
- Copy the token immediately — you won't see it again

### Go to Manage Jenkins → Credentials → System → Global credentials → Add Credentials
- Kind: Secret text
- Secret: paste your SonarQube token
- ID: sonarqube-token
- Description: SonarQube Token
- Click Create

- <img width="958" height="469" alt="sonaqueb login-page" src="https://github.com/user-attachments/assets/5579810c-3e8d-499d-9aa0-1550d6488522" />

<img width="959" height="407" alt="sonaqueb-token" src="https://github.com/user-attachments/assets/1003ca5d-2a45-4b51-bf46-1c41219007db" />

### Go to Manage Jenkins → System and scroll down to SonarQube servers section:
- Check Environment variables checkbox
- Click Add SonarQube
- Name: SonarQube
- Server URL: http://34.245.151.213:9000
- Server authentication token: select sonarqube-token

### Go to Manage Jenkins → Tools and scroll down to SonarQube Scanner section:
- Click Add SonarQube Scanner
- Name: SonarScanner
- Check Install automatically
- Select the latest version

### Go to Java-App job → Configure → Build Steps
- Click Add build step
- Select Execute SonarQube Scanner
- Analysis properties:

Now add the SonarQube analysis step to the Jenkins job:

Go to Java-App job → Configure → Build Steps

Click Add build step
Select Execute SonarQube Scanner
Analysis properties:
sonar.projectKey=Java-Login-App
sonar.projectName=Java-Login-App
sonar.projectVersion=1.0
sonar.sources=Java-Login-App/src/main/java
sonar.java.binaries=Java-Login-App/target/classes
Make sure this step is after the Maven clean package step.

<img width="946" height="428" alt="Screenshot 2026-07-03 121811" src="https://github.com/user-attachments/assets/92287b36-c92b-4044-94bb-6378512dd090" />

- <img width="947" height="434" alt="SAST-SUCESSFUL" src="https://github.com/user-attachments/assets/11f612af-7f7d-418b-a9af-1b37207c7c71" />

# Nexus integration 
- sudo systemctl restart nexus
- sudo cat /opt/sonatype-work/nexus3/admin.password

### Good. Now create a Maven hosted repository for storing the WAR artifacts:
- Click Settings (gear icon) → Repositories → Create repository
- Select maven2 (hosted)
- Name: maven-releases
- Version policy: Release
- Deployment policy: Allow redeploy
- Click Create repository

### Now add Nexus credentials to Jenkins:
Go to Manage Jenkins → Credentials → System → Global credentials → Add Credentials
- Kind: Username with password
- Username: admin
- Pas-sword: 327c5634-d4fe-4a55-a0b5-5979e382bd7d
- ID: nexus-credentials
- Description: Nexus Credentials
- Click Creat

### Now we need to update the pom.xml in your repo to add the Nexus distributionManagement section so Maven knows where to publish the WAR.
- Go to your GitHub repo → Java-Login-App/pom.xml and add this inside the
-  <project> tag:
xml<distributionManagement>
    <repository>
        <id>nexus-releases</id>
        <url>http://18.201.11.171:8081/repository/maven-releases/</url>
    </repository>

    Edit it directly on GitHub:

### Go to Java-Login-App/pom.xml
Click the pencil icon to edit
Find and replace the distributionManagement section
Commit the changes
</distributionManagement>


- sudo mkdir -p /var/lib/jenkins/.m2
- sudo tee /var/lib/jenkins/.m2/settings.xml << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<settings>
  <servers>
    <server>
      <id>nexus-releases</id>
      <username>admin</username>
      <password>Boluwatife</password>
    </server>
  </servers>
</settings>
EOF
 sudo chown -R jenkins:jenkins /var/lib/jenkins/.m2
 sudo ls -la /var/lib/jenkins/.m2/
 sudo cat /var/lib/jenkins/.m2/settings.xml


<img width="959" height="439" alt="Nexus-build-sucessful" src="https://github.com/user-attachments/assets/5cc2ae0a-23f3-4efd-9717-bbec31380ab1" />

<img width="938" height="407" alt="nexus-sonarquwb sucessful" src="https://github.com/user-attachments/assets/b95c81db-e353-436a-920a-91d15b4d0d7c" />

<img width="958" height="437" alt="image" src="https://github.com/user-attachments/assets/fe20b36e-f815-4ab5-9fb9-e6433539a252" />

## The webhook is configured correctly. Now let's verify it actually triggers Jenkins automatically.
Make a small change to your repo on GitHub:

Go to https://github.com/Davix4u/Jenlins-cicd
Click on README.md
Click the pencil icon to edit
Add a line like # Test webhook trigger
Click Commit changes

# Test webhook trigger



















  








  








