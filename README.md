# Jenkins-Container-Dockerfile



Step 1: CREATE JENKINS CONTAINER FROM DOCKERFILE:
==================================================
1. place the Dockerfile in a new directory and run docker build command.
docker build -t jenkins:1.0 ./

2. Run the jenkins container.
docker run -tid --name myjenkins jenkins:1.0

Step-2: JENKINS USER ROOT ACCESS:
=================================
1. Provide jenkins user paswordless sudo access.
docker exec -ti myjenkins bash
apt-get install sudo -y
echo "jenkins ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/jenkins

2. Check if jenkins has root access without password.
sudo su - jenkins     
sudo apt install ssh

Step -3: INSTALL JENKINS JOB BUILDER:
=====================================
1. login to jenkins container and install jenkins job builder.
docker exec -ti myjenkins bash
apt-get install python-pip
pip install jenkins-job-builder

2. configure jenkins job builder.
mkdir /etc/jenkins_jobs
vim /etc/jenkins_jobs/jenkins_jobs.ini  // Paste the below in the jenkins_jobs.ini file with username and password

[jenkins]
user=admin
password=6a7265ca0568489eaa9c94219923fd23
url=http://localhost:8080
query_plugins_info=False

Step 4,5,6: CREATE TWO JOB DEFINATIONS:
=======================================
1. Create a jobs folder in the home directory.
mkdir jobs

2. Create first job defination.
vim jobs/job1.yaml

- job:
        name: ls_job
        description: "job defination 1"
        project-type: freestyle
        builders:
                - shell: 'ls -la'

3. Create second job defination.
vim jobs/job2.yaml

- job:
        name: job2
        description: "Job defination 2"
        parameters:
            - choice:
                name: terraform command
                choices:
                        - terraform plan
                        - terraform apply
                        - terraform destroy  

3. Apply the jenkins jobs using jenkins job builder.
jenkins-jobs update jobs


Step - 7: EXPLAINATION FOR GIT INTEGRATION:
===========================================

To integrate it with git, we can use either of the below:
1. Install jenkins git plugin.

Go to Jenkins > Manage Jenkins > Manage Plugin
Go to available tab, search for 'git' and install the 'Git plugin'.

2. Integrate Jenkins with your created jobs:

Go to your job and scroll to the 'Source Code Management' section.
Select 'Git' in the choices.
Provide the git 'Repository URL' of your git repo.
To add Jenkins Credentials, select the 'Add' button drop-down and select 'Jenkins' for adding credentials.
Provide the git username and password and click on 'add' button.
Select the added credentials from the dropdown.
Update 'Branches to build' if required. 
 




Note: The above explanation is for Dockerfile1. Dockerfile.txt have minor changes.




