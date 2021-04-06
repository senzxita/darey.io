# WEBSTACK IMPLEMENTATION (LAMPSTACK) IN AWS
I will be deploying a LAMP stack on AWS using EC2 instance
LAMP means: - Linux, Apache2, Mysql, Python/Perl/Php

The first step is to create an EC2 instance after logging in to AWS Management console
![image](https://user-images.githubusercontent.com/20463821/113478715-856b6780-9482-11eb-9d80-7d67d928cb10.png)

This instance inbound rules has TCP running on port 22 (to allow ssh connection) and port 80 (default HTTP access) and has it source on 0.0.0.0/0. This signifies that any ip address can access this instance.
![image](https://user-images.githubusercontent.com/20463821/113478800-0c204480-9483-11eb-8478-00fe0c390ed8.png)

It is time to connect the running instance to our local machine, such that we can communicate with the instance from our local machine. During the creation of the instance you are expected to have saved the key-pair on your local machine, this saved key is what will be used to initiate the connection. On my linux machine, the key-pair is saved as aws-key.pem. using `ssh -i <key-pair file/directory> ubuntu@Public-IP`. Also take note of the public IP of the instance created.

On your Linux machine:
![image](https://user-images.githubusercontent.com/20463821/113479009-48a07000-9484-11eb-9c23-67951efc4b59.png)

Run `sudo apt update` to update the necessary packages.
Afterwards, install apache2 using `sudo apt install apache2`, confirm apache2 service is running using `sudo systemctl status apache2`. Enable firewall using `sudo ufw enable` after add http rule in the security group. Then allow apache traffic on the server `sudo ufw allow 'apache`
![image](https://user-images.githubusercontent.com/20463821/113479284-17c13a80-9486-11eb-8502-79527c5b83b6.png)

Install Mysql and run the configuration script, using `sudo apt install mysl-server` and `sudo mysql_secure_installation`
![image](https://user-images.githubusercontent.com/20463821/113479360-8b634780-9486-11eb-9b49-2cbcaf1b161e.png)

To install php packages using `sudo apt install php libapache2-mod-php php-mysql`
Enable PHP on website
![php](https://user-images.githubusercontent.com/20463821/113479416-edbc4800-9486-11eb-9cae-27b9fa0fb3fe.PNG)

Congratulations on your first Webstack implementation, more accomplishments!!!
