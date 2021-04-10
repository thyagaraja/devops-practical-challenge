## Ansible Playbook to Inatall and deploy nginx

Launch an EC2 Instances with (Amazon Linux 2 AMI) Ansible Control node.

SSH into Ansible control node and Install Ansible 
``` ssh -i "devops-practice.pem" ec2-user@100.26.29.69 ```

Switch to root user ```sudo su```

Intially update the all packages

```sudo yum update -y ```

Create ansible user 
``` sudo useradd ansible ```

Add ansible user to sudoers file
visudo file add ```ansible ALL=(ALL)       NOPASSWD: ALL```

Switch to ansible user
``` su - ansible ```
 
Install latest version of Ansible 
``` sudo yum install ansible -y``` or ``` sudo amazon-linux-extras install ansible2```

Check the ansible version

``` ansible --version ```

ansible 2.9.18

Setting up passwordless connection between Ansible control server and  ansible clinet nginx server
Execute on control node:
```
ssh-keygen -t rsa -b 4096
ssh-copy-id nginx@hostname ```

Set up our inventory file at /etc/ansible/hosts

[nginx]
172.31.28.163 ansible_user=nginx ansible_connection=ssh

[nginx:vars]
ansible_python_interpreter=/usr/bin/python3

We add the python interpreter to make sure we don't use older python version

Test connectivity using ping module: ```ansible 172.31.28.163 -m ping```

Since executing install commands on remote will require sudo privileges, setting up our user to be in sudo group by editing visudo in host

To validate the playbook use: 
```ansible-playbook sudoers.yml --syntax-check
   ansible-playbook nginx_install.yml --syntax-check
   ansible-playbook nginx_deploye.yml --syntax-check
```

Execute using:```ansible-playbook sudoers.yml -b -K```
-b flag is to become root and -K is to provide one time auth password

Install nginx using Ansible:
``` ansible-playbook nginx_install.yml -b ```

nginx should now be running in the remote server

Playbook  to deploye custom content to the remote server: ```ansible-playbook nginx_deploye.yml```



## Cron job to check if application is runnig and if not send and email

We will run a cron job which will run regularly every 5 mins and check if server returns a 200 status code if not, it will send and email
server-check.py holds the python code to do that task
edit crontab and add the entry to execute the python script
```
*/5 * * * * nginx `/usr/bin/python3 /home/nginx/server-check.py &> log.out`
```
Here we run our python file every 5 mins and log its stdout and stderr to log

if cron fails check ```tail -f /var/log/syslog``` for logs
