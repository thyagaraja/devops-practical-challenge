## Basic Nginx Setup on AWS EC2

Launch an EC2 Instances with (Amazon Linux 2 AMI) Ansible Client (Nginx).

SSH into Ansible client node and Install Nginx 
``` ssh -i "nginx.pem" ec2-user@IP ```

Switch to root user ```sudo su```

Intially update all packages

``` yum update -y ```

Create nginx user 
``` useradd nginx ```
Add nginx user to sudoers file
visudo file add ``` nginx ALL=(ALL)       NOPASSWD: ALL ```

Switch to nginx user
``` su - nginx ```
 
Install latest version of Nginx
``` sudo yum install nginx -y``` or ``` sudo amazon-linux-extras install nginx1 -y```

Check the Nginx version
``` nginx -v ```
nginx version: nginx/1.18.0

To Start service:
```sudo service nginx start```

To Check the status:
```sudo service nginx status```

Verify if default nginx page shows up:
```curl http://localhost:80```

This should return the HTML content of the page

Deploye the custom content:

Create a directory with our custom webpage at /home/nginx/static-site:

Update default nginx.conf file:

update custom site path to root directory of nginx.conf file

``` root   /home/nginx/static-site; ```

Restart nginx and verify
```
sudo service restart nginx
curl http://localhost:80
```
This should be returning our custom webpage

``` http://34.207.114.194:80 ```
