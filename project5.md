## CLIENT-SERVER ARCHITECTURE USING MYSQL
- Create two EC2-instances on AWS console, One named `mysql server` and the other is names `mysql client`, ensure port 22 is open.

![image](https://user-images.githubusercontent.com/20463821/116816950-17d35980-ab5c-11eb-8712-e3330616591c.png)

- Connect to both instances using ssh using `ssh -i <awskey> ubuntu@instance-public-ip`
- For mysql server 

![image](https://user-images.githubusercontent.com/20463821/116817093-b6f85100-ab5c-11eb-92f1-bb38c508b3ee.png)

- For mysql client


![image](https://user-images.githubusercontent.com/20463821/116817117-cbd4e480-ab5c-11eb-814b-ddb437a66d8d.png)

- Run `sudo apt update && sudo apt upgrade` on both instances and install mysql server software using
```
sudo apt install mysql-server
sudo mysql_secure_installation
```
- Open port 3306 on mysql server instance on AWS console. to allow mysql client instance access the port, input its private IP4 address under Source

![image](https://user-images.githubusercontent.com/20463821/116817327-b3b19500-ab5d-11eb-900c-9c5fbf9371e2.png)
## On Mysql Server Instance
- Configure mysql server to allow connections from remote hosts, open mysql.cnf file and change the bind address value from *127.0.0.1* to 0.0.0.0, using the command `sudo vi/etc/mysql/mysql.conf.d/mysqld.cnf`

![image](https://user-images.githubusercontent.com/20463821/116817462-5f5ae500-ab5e-11eb-8312-741b0cf26f03.png)

- Start mysql server service using `sudo systemctl start mysql` on the server instance.
- To confirm the service is running use the command `sudo systemctl status mysql`
- Start mysql shell using `sudo mysql`
- Create a user using `CREATE USER '<username>'@'<client-private-ip>' IDENTIFIED WITH mysql_native_password BY '<password>';`

![image](https://user-images.githubusercontent.com/20463821/116918646-15e6c480-ac48-11eb-87ee-30ba59f03914.png)

- Grant all privileges to any database to the user `GRANT ALL PRIVILEGES ON *.* TO '<username>@'<client-private-ip>';`
## On Mysql Client Instance
- To connect to Mysql server Database Engine use `sudo mysql -u <username> -h <server-private-ip> -p`, you will be prompted for a password afterwards. 
- To show the databases, use `show databases;`


![image](https://user-images.githubusercontent.com/20463821/116918795-462e6300-ac48-11eb-9a92-75530cd70e83.png)
