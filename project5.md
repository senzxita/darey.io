## CLIENT-SERVER ARCHITECTURE USING MYSQL
- Create two EC2-instances on AWS console, One named `mysql server` and the other is names `mysql client`, ensure port 22 is open.

![image](https://user-images.githubusercontent.com/20463821/116816950-17d35980-ab5c-11eb-8712-e3330616591c.png)

- Connect to both instances using ssh using `ssh -i <awskey> ubuntu@instance-public-ip`
- For mysql server 

![image](https://user-images.githubusercontent.com/20463821/116817093-b6f85100-ab5c-11eb-92f1-bb38c508b3ee.png)

- For mysql client


![image](https://user-images.githubusercontent.com/20463821/116817117-cbd4e480-ab5c-11eb-814b-ddb437a66d8d.png)

- Run `sudo apt update && sudo apt upgrade` on both instances
- On mysql server instance, install mysql server software using 
```
sudo apt install mysql-server
sudo mysql_secure_installation
```
- On mysql client instance, install mysql client software `sudo apt install mysql-client`
- Open port 3306 on mysql server instance on AWS console. to allow mysql client instance access the port, input its private IP4 address under Source

![image](https://user-images.githubusercontent.com/20463821/116817327-b3b19500-ab5d-11eb-900c-9c5fbf9371e2.png)

- Configure mysql server to allow connections from remote hosts, open mysql.cnf file and change the bind address value from *127.0.0.1* to 0.0.0.0, using the command `sudo vi/etc/mysql/mysql.conf.d/mysqld.cnf`

![image](https://user-images.githubusercontent.com/20463821/116817462-5f5ae500-ab5e-11eb-8312-741b0cf26f03.png)

