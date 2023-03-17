## The Project Setup with Continous Integration

### Step 1 - Install and Configure jenkins Server. RHEL-8 Server will used for the configuration of the Jenkins Server. The RHEL-8.4.0_HVM-20230125-x86_64-35-Hourly2-GP2 AMI image was used as there was issue using RHEL-9 over SSH connection

### Next, the JDK installation (Jenkins is a Java-based app) and also Jenkins Installation
`sudo yum update`

`yum install wget`

`sudo wget -O /etc/yum.repos.d/jenkins.repo \ https://pkg.jenkins.io/redhat-stable/jenkins.repo`

`sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`

`sudo yum upgrade`

`yum install java-11-openjdk-devel`

`sudo yum install jenkins`

![jenkins-install](/images/Jenkins-status.PNG)

### Port 8080 was added to the inbound security rules of the Jenkins server to allow connection and be accessed via the web using the Private IP of the server
`http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080`

![jenkins-setup](/images/jenkins-setup.PNG)

### The administrator password was retrieved from the server and the Jenkins setup was completed.
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

![jenkins-custom](/images/jenkins-custom.PNG)

![jenkins-crerate-admin](/images/jenkins-create-admin.PNG)

![jenkins-url](/images/jenkins-url.PNG)



### Step 2 - Jenkins Configuration to retrieve source codes from Github using Webhooks

![webhook](/images/webhook-created.PNG)

### A new project was created on the Jenkins platform and it was configured to connect to the Github repository using the username/password credential method so the repo files can be accessed through Jenkins. Then a test build was done manually to confirm the configuration. 

![console-build](/images/console-build.PNG)


### Next, the Build configuration was done by triggering Github webhook and the post-build actions to archive artifacts resulted from a build. To thie end, changes were made to the repo file locally and pushed to the main branch in the github repo in order to see the new build automaticalluy being launched by the webhook.


![build-change](/images/build-change.PNG)



### Step 3 - Jenkins configuration to copy files to NFS server via SSH. RHEL-8 Instance was used to setup the Jenkins.

### A plug in Publish-over-SSH was installed and the project on the Jenkins was configured to copy artifacts using the following:

![ssh-publish](/images/jenkins-publish-ssh-install.PNG)

### - Content of Private key for the server instance
### - Name of server
### - Host name (Private IP address of the NFS server)
### - Username (ec2-user)
### - Remote directory which was the mounting point used by webservers (/mnt/apps) 

![jenkins-ssh](/images/jenkins-ssh-test.PNG)


### Next, another post-build action was configured on the project to send build artifacts over SSH. Then another change was made to the project file to test the configuration done.

![build-change](/images/ssh-conn.PNG)


### Finally, to confirm the files in /mnt/apps had been updated, the Readme file that was changed locally was checked via the command line on the NFS server

![ssh-readme](/images/ssh-view-readme.PNG)