## 1.0 Launch an Amazon Linux 2 instance and Configure Jenkins
- name: Jenkins-Master
- machine type: t2.medium
- image/ami: Amazon Linux 2
- Key pair: Select or Create
- Security group ports: 8080, 22
 
 ### Login and Install Jenkins
 ```
#!/bin/bash
sudo su
yum update –y
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
amazon-linux-extras install epel -y
amazon-linux-extras install java-openjdk11 -y
yum install jenkins -y
systemctl enable jenkins
systemctl start jenkins

## Installing Git
yum install git -y
 ```

### Configure Master and Clinet Configuration
- Click on "Manage Jenkins" >> Click "Nodes and Cloud" >> Click "New Node"
- Node Name: Maven-Build-Env and Select "Permanent Agent" >> Click "CREATE"

1. Configure "Maven-Build-Env"
- Name:                  Maven-Build-Env
- Number of Executors:   5 (for example, maximum jobs to execute at a time)
- Remote root directory: /opt/maven-builds
- Labels:                Maven-Build-Env
- Usage:                 Use this node as much as possible
- Launch method:         Launch agents via SSH
    - Host:   Provide IP of Maven-Build-Server
    - Credentials: 
        - Login to Maven VM
        - Run the following commands
            - sudo su
            - passwd root
            - provide the password as "root", "root"
            - vi /etc/ssh/sshd_config       (:/PasswordAuthentication)
            - systemctl restart sshd
    - Credentials:
        - Click on `Add / Jenkins` and Select `Username and Password`
        - Username: `root`
        - Password: `root`
        - ID: `Maven-Build-Env-Credential`
        - Save
        - Credentials: Select `Maven-Build-Env-Credential`
    - Host Key Verification Strategy: `Non Verifying Verification Strategy`
    - Availability: `Keep this agent online as much as possible`
    - Click `SAVE`

    - NOTE: Make sure the `Agent Status` shows `Agent successfully connected and online` on the Logs
    - NOTE: Repeat the process for adding additional Nodes






