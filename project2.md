# WEBSTACK IMPLEMENTATION (LEMPSTACK) IN AWS
In my first project I spinned up an EC2 instance using Amazon Linux 2 AMI base image. This time I will create an EC2 instance using Ubuntu version 20.04 as a base image to deploy LEMP stack, LEMP means: Linux, Nginx, MySQL, PHP/Python/Perl
After successfully launching the instance, time to connect to the instance through Git Bash unlike using the linux terminal in the first project. Made use of the security group that was created for project1.

![image](https://user-images.githubusercontent.com/20463821/113768678-03946c00-9718-11eb-837d-4ed0cf477cc3.png) 

To connect use `ssh -i <keypairfile> ubuntu@<instance-public-IP>` 

![image](https://user-images.githubusercontent.com/20463821/113774150-ced7e300-971e-11eb-8d3c-3ca13167d691.png)

![image](https://user-images.githubusercontent.com/20463821/113767979-3427d600-9717-11eb-8ad5-1e0706f5d98d.png)

Run `sudo apt update` to install necessary updates, then install nginx on the instance using the command `sudo apt install nginx`, afterwards confirm the nginx service is running using `sudo systemctl status nginx` or `sudo service nginx status`. If nginx is not running , you can start the service using `sudo service nginx start`.

![image](https://user-images.githubusercontent.com/20463821/113775819-25462100-9721-11eb-837e-b098043c0a64.png)

Add HTTP rule in the security group of the instance to open port 80 

![image](https://user-images.githubusercontent.com/20463821/113776872-48bd9b80-9722-11eb-80fd-45eccc29dc78.png)

Enable firewall using `sudo ufw enable`, check the status `sudo ufw status`, check the available applications running `sudo ufw app list`

![image](https://user-images.githubusercontent.com/20463821/113779333-79530480-9725-11eb-80ca-9ebbdfa0db51.png)

Let's try to access nginx server from the terminal using `curl http://localhost:80` 

![image](https://user-images.githubusercontent.com/20463821/113777457-03e63480-9723-11eb-9b0d-4aac43c7d99a.png)

or using EC2 instance IPv4 `curl http:172.31.53.84:80`

![image](https://user-images.githubusercontent.com/20463821/113778021-c9c96280-9723-11eb-90b2-e2e7e3a50840.png)

Now let's access nginx landing page using the browser
