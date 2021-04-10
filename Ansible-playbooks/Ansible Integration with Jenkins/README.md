## Jenkins integration to run Ansible playbook

Install Jenkins on the control node:

Create Jenkins user and switch to jenkins user
``` useradd jenkins ```

Add jenkins user to sudoers file
visudo file add ``` jenkins ALL=(ALL)       NOPASSWD: ALL```

Switch to jenkins user
``` su - jenkins ```
Install java packages and remove the oldest version of java if any:
``` sudo yum install java-1.8.0 
    sudo yum remove java-1.7.0-openjdk ```

Add the Jenkins repo:
``` sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo ```

Import the key file:
``` sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key ```

Install Jenkins using the below command:
``` sudo yum install jenkins -y ```

Start the Jenkins service:
``` sudo service jenkins start ```

Open the browser and hit the public IP along with the 8080 port.

Once jenkins up and running, install Ansible plugin from Manage Jenkins --> Manage Plugins
And then configure with Global Tool Configuration setting the ansible home dir

A Jenkinsfile is a text file that contains the definition of a Jenkins Pipeline and is checked into source control. 
Consider the following Pipeline which implements a basic two-stage continuous delivery pipeline.
create jenkins file like below
```
pipeline {
    agent any
    stages{
        stage('SCM Checkout'){
            steps{
                git 'https://github.com/thyagaraja/devops-practical-challenge'
            }
        }
        stage('Execute Ansible PB'){
            steps{
                ansiblePlaybook become: true, credentialsId: '79827e329-3456-7g673l-76589-e22242223', disableHostKeyChecking: true, installation: 'ansible', inventory: 'playbooks/inv.ini', playbook: 'playbooks/nginx.yml'
            }
        }
    }
}
```

The credentialsId here is built with the Pipeline syntax where node access config is set and paths to inventory file and the actual playbook need to be saved

We can run the build and this would go ahead  execute playbook and have nginx deployed on the managed node.