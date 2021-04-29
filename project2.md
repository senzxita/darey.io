# WEBSTACK IMPLEMENTATION (LEMPSTACK) IN AWS
In my first project I spinned up an EC2 instance using Amazon Linux 2 AMI base image. This time I will create an EC2 instance using Ubuntu version 20.04 as a base image to deploy LEMP stack, LEMP means: Linux, Nginx, MySQL, PHP/Python/Perl
After successfully launching the instance, time to connect to the instance through Git Bash unlike using the linux terminal in the first project. Made use of the security group that was created for project1.

![image](https://user-images.githubusercontent.com/20463821/113768678-03946c00-9718-11eb-837d-4ed0cf477cc3.png) 

To connect use `ssh -i <keypairfile> ubuntu@<instance-public-IP>` 

![image](https://user-images.githubusercontent.com/20463821/113774150-ced7e300-971e-11eb-8d3c-3ca13167d691.png)

![image](https://user-images.githubusercontent.com/20463821/113767979-3427d600-9717-11eb-8ad5-1e0706f5d98d.png)

## Install Nginx

- Run `sudo apt update` to install necessary updates, then install nginx on the instance using the command `sudo apt install nginx`, afterwards confirm the nginx service is running using `sudo systemctl status nginx` or `sudo service nginx status`. If nginx is not running , you can start the service using `sudo service nginx start`.

![image](https://user-images.githubusercontent.com/20463821/113775819-25462100-9721-11eb-837e-b098043c0a64.png)

- Add HTTP rule in the security group of the instance to open port 80 

![image](https://user-images.githubusercontent.com/20463821/113776872-48bd9b80-9722-11eb-80fd-45eccc29dc78.png)

- Enable firewall using `sudo ufw enable`, check the status `sudo ufw status`, check the available applications running `sudo ufw app list`

![image](https://user-images.githubusercontent.com/20463821/113779333-79530480-9725-11eb-80ca-9ebbdfa0db51.png)

- Let's try to access nginx server from the terminal using `curl http://localhost:80` 

![image](https://user-images.githubusercontent.com/20463821/113777457-03e63480-9723-11eb-9b0d-4aac43c7d99a.png)

or using EC2 instance IPv4 `curl http:172.31.53.84:80`

![image](https://user-images.githubusercontent.com/20463821/113778021-c9c96280-9723-11eb-90b2-e2e7e3a50840.png)

- Now, let's access nginx landing page using the browser using the EC2 public instance

![image](https://user-images.githubusercontent.com/20463821/116458055-10f6cf00-a85c-11eb-8b09-b3c28a135d75.png)

## Install MySQL

- Install MySQL using `sudo apt install mysql-server` and run the installation script  `sudo mysql-secure-installation` and accept the follow the prompt accordingly, login to the MySQL `sudo mysql` afterwards.

![image](https://user-images.githubusercontent.com/20463821/116459000-2f10ff00-a85d-11eb-9614-1f4418928600.png)

## Install PHP and Configure Nginx to use PHP processor
To Install PHP, one has to install *php-fpm*; PHP fastCGT process manager and *php-mysql* using `sudo apt install php-fpm php-mysql`

![image](https://user-images.githubusercontent.com/20463821/116460180-a7c48b00-a85e-11eb-8553-ace02ec35d52.png)

- Create a root web directory to use for the project `sudo mkdir /var/www/projectLEMP` and assign ownership to the directory `sudo chown -R ubuntu:ubuntu /var/www/projectLEMP`

- Create a configuration file in Niginx/sites-available `sudo nano /etc/nginx/sites-available/projectLEMP` and add some configurations to it.

![image](https://user-images.githubusercontent.com/20463821/116462682-a21c7480-a861-11eb-9dda-775cf46aec76.png)

![image](https://user-images.githubusercontent.com/20463821/116462724-b496ae00-a861-11eb-9501-d7ec661edf88.png)

- Activate the configuration written in sites-available/projectLEMP file, link to the config file in Nginx/sites-enabled `sudo ln -s /etc/nginx/sites-available/project/ /etc/nginx/sites-enabled/`, this is to tell Nginx to use the file when reloaded.


- Test the configuration for any syntax error `sudo nginx -t`

![image](https://user-images.githubusercontent.com/20463821/116463245-61712b00-a862-11eb-9108-614b5c1b8dff.png)


- Disable default Nginx host listening on port 80 in sites-enabled/default `sudo unlink /etc/nginx/sites-enabled/default` and reload Nginx service `sudo systemctl reload nginx` 

![image](https://user-images.githubusercontent.com/20463821/116467005-1d345980-a867-11eb-85f7-8cce73255fbc.png)

- Create index.html file in /var/www/projectLEMP diretory and go to browser to view the page using public Ip http://52.87.255.3:80
![image](https://user-images.githubusercontent.com/20463821/116470559-7e5e2c00-a86b-11eb-85bc-f70a332787b2.png)


- Using public host name http://ec2-52-87-255-3.compute-1.amazonaws.com

![image](https://user-images.githubusercontent.com/20463821/116468725-20304980-a869-11eb-9c90-91aa1c0bd376.png)

## Test PHP with Nginx
- Let index.php take precedence over index.html in sites-available

![image](https://user-images.githubusercontent.com/20463821/116470000-c2046600-a86a-11eb-911f-21f1f84ce671.png)

- Create a info.php file in /var/www/projectLEMP directory to validate Nginx can get .php files to PHP processor. Access the page on your browser using your DNS name or Instance public address `http://instance-public-ip-add/info.php`.


![image](https://user-images.githubusercontent.com/20463821/116470692-abaada00-a86b-11eb-9355-37fc611504e2.png)

- Ensure to always remove the info.php page because it contains sensitive information about the PHP environment, using `sudo rm /var/www/projectLEMP/info.php`


## Retrieve Data from MySQL with PHP
- Login to MySQL, create a database and a user; grant user full priviledges on the database using mysql_native_password as the default authentication method, exit afterwards.

![image](https://user-images.githubusercontent.com/20463821/116485733-d739be80-a883-11eb-80bd-40497f93a9dc.png)

- Login to MySQL console using the user credentials created and confirm you access the database.
![image](https://user-images.githubusercontent.com/20463821/116485912-53340680-a884-11eb-9b3c-c63b48993570.png)

- Create a table and add items to the table and view table.
![image](https://user-images.githubusercontent.com/20463821/116486567-dbff7200-a885-11eb-8cab-4372484b68a8.png)

![image](https://user-images.githubusercontent.com/20463821/116486580-e7529d80-a885-11eb-8f74-68fed8b0eb23.png)

- Create a PHP script that connects to MySQL and query for content in the project directory `nano /var/www/projectLEMP/todo_list.php`, save and close after editing.
- Access the page from browser using http://DNS-or-instance-ip/todo_list.php ie http://52.87.255.3/todo_list.php

![image](https://user-images.githubusercontent.com/20463821/116487071-056ccd80-a887-11eb-96ad-8e67d1efd0da.png)

 









