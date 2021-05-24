
Will repeat project 7 steps but this iime using Microsoft Azure
- Signup/Login to Microsoft Azure Portal and select *Virtual machines* 

![image](https://user-images.githubusercontent.com/20463821/119374969-28d23f00-bcb2-11eb-9f78-ff6d513fb184.png)

- Click on *Add* Dropdown menu and select *Virtual machine* 

![image](https://user-images.githubusercontent.com/20463821/119375332-98e0c500-bcb2-11eb-85dd-ad71dbf4389b.png)

- Create a NFS-Server vm with Red Hat Enterprise Linux (RHEL) Os base image and 8 GiB storage size, ensure to create ssh key if you do not have any

![image](https://user-images.githubusercontent.com/20463821/119376416-d1cd6980-bcb3-11eb-8d2b-25e46c1d8be2.png)

- Open port 22 to enable SSH access

![image](https://user-images.githubusercontent.com/20463821/119376983-7b145f80-bcb4-11eb-8ff8-d163fae67af2.png)

- Click on the **Review + create** Tab and select the *Create* button after configuration has passed the validation test.

![image](https://user-images.githubusercontent.com/20463821/119377611-3937e900-bcb5-11eb-8de6-793b46b36a27.png)

- If you indicated during the configuration that you will like to create a ssh key, download the private ssh key

![image](https://user-images.githubusercontent.com/20463821/119377811-72705900-bcb5-11eb-8ff6-21803846976d.png)

- NFS-Server VM has been created successfully

![image](https://user-images.githubusercontent.com/20463821/119378352-18bc5e80-bcb6-11eb-9e1b-914115eff3ab.png)

- To create and attach 3 disks to NFS-Server, select *Disks* from the menu on the lefthand side of the screen and select **Create and attach a new disk**

![image](https://user-images.githubusercontent.com/20463821/119378904-b1eb7500-bcb6-11eb-9f9c-cf24d140f59f.png)

- Each disk with 10GiB SSD storage size, click on the **Save** button afterwards

![image](https://user-images.githubusercontent.com/20463821/119379315-40f88d00-bcb7-11eb-83c0-daf236c8aa37.png)

- Connect to the VM via SSH using `ssh -i <Azurekeyfile> <azureusername>@VM-public-ip`

![image](https://user-images.githubusercontent.com/20463821/119383582-6a1b1c80-bcbb-11eb-9bd9-e363973bba6b.png)

- In the terminal, use `lsblk` to inspect the disks attached to the server and also the newly created disks `sdc`, `sdd` and `sde`

![image](https://user-images.githubusercontent.com/20463821/119384048-080ee700-bcbc-11eb-8559-d4c6eb9c7616.png)

- Create single partition for the 3 disks using gdisk utility `sudo gdisk /dev/sdc`, `sudo gdisk /dev/sdd` and `sudo gdisk /dev/sde`

![image](https://user-images.githubusercontent.com/20463821/119384361-81a6d500-bcbc-11eb-94b4-add50ab5f573.png)

![image](https://user-images.githubusercontent.com/20463821/119384483-b024b000-bcbc-11eb-87d0-906fe7b2e68e.png)

- Use `lsblk` to check the newly configured partitions

![image](https://user-images.githubusercontent.com/20463821/119384861-350fc980-bcbd-11eb-9504-0e3ca5a9b6d2.png)

- Install lvm2 package `sudo yum install lvm2`
- Check the available partitions on the server `sudo lvmdiskscan`

![image](https://user-images.githubusercontent.com/20463821/119385079-8ddf6200-bcbd-11eb-975a-5e350a2f6b37.png)

- Mark the 3 disks as physical volumes to be used by LVM using **pvcreate** utility

![image](https://user-images.githubusercontent.com/20463821/119385249-ca12c280-bcbd-11eb-869e-816e643aeec6.png)

- Verify the physical volumes have been created using `sudo pvs`

![image](https://user-images.githubusercontent.com/20463821/119385396-f8909d80-bcbd-11eb-8b07-c1a510c61977.png)

- Create a "webserver-vg" volume group, using "vgcreate" utility and add the 3 physical volumes to the volume group created

![image](https://user-images.githubusercontent.com/20463821/119385661-5ae99e00-bcbe-11eb-90ee-3454c0b79fc9.png)

- Verify the Volume group has been created successfully `sudo vgs`

![image](https://user-images.githubusercontent.com/20463821/119393390-c7699a80-bcc8-11eb-98d7-02b090cc46e4.png)

- Create 3 logical volumes namely: `lv-opt`, `lv-apps` and `lv-logs` using "lvcreate" utility. Each lv will take a portion of the physical volume which is 10 GiB, where lv-opt and lv-logs will take 3.3 GiB each and lv-apps will take the rest of the available space which is 3.4 GiB

![image](https://user-images.githubusercontent.com/20463821/119394757-79ee2d00-bcca-11eb-86e5-2922c69ab5bf.png)

- Confirm the logical volumes have been created successfully `sudo lvs`

![image](https://user-images.githubusercontent.com/20463821/119394855-9be7af80-bcca-11eb-9802-3a0b378da23d.png)

- Verify the entire setup `sudo vgdisplay -v`

![image](https://user-images.githubusercontent.com/20463821/119394999-d2252f00-bcca-11eb-97a5-92dc7319d39d.png)

- Format each of the logical volume with `xfs` file system using `mkfs.xfs`

![image](https://user-images.githubusercontent.com/20463821/119395255-25977d00-bccb-11eb-9cf8-c1badf137079.png)

- Create a "/mnt directory", where you can mount the logical volumes created, using touch command to create apps, logs and opt files in the mnt directory

![image](https://user-images.githubusercontent.com/20463821/119395505-82933300-bccb-11eb-9fd7-5d78c72caf4d.png)

- Mount /mnt/apps on lv-apps logical volume `sudo mount /dev/webserver-vg/lv-apps /mnt/apps` and /mnt/opt on lv-opt logical volume `sudo mount /dev/webserver-vg/lv-opt /mnt/opt`

![image](https://user-images.githubusercontent.com/20463821/119395680-c5eda180-bccb-11eb-9be6-d8be8ff3faef.png)

- Backup all files in the log directory "/var/log" into "/mnt/logs" using rsync utility `sudo rsync -av /var/log/. /mnt/logs`

![image](https://user-images.githubusercontent.com/20463821/119395852-02210200-bccc-11eb-9102-44b1d08a6a2c.png)

- Mount /var/log on lv-logs logical volume `sudo mount /dev/webserver-vg/lv-logs /var/log`
- Restore log files in /mnt/logs directory back into /var/log directory `sudo rsync -av /mnt/logs/. /var/log`
- Use `sudo blkid` to find the UUID of each device and copy to a notepad

![image](https://user-images.githubusercontent.com/20463821/119396343-a440ea00-bccc-11eb-82cb-3fab2968c868.png)

- To make the mount configuration persist after restart, enter the values of the UUID copied in /etc/fstab using `sudo vi /etc/fstab`

![image](https://user-images.githubusercontent.com/20463821/119397251-c2f3b080-bccd-11eb-83ef-b1ebd1376aa9.png)

- Test the configuration `sudo mount -a` and reload daemon `sudo systemctl daemon-reload`
- Verify setup, run `df -h`

 ![image](https://user-images.githubusercontent.com/20463821/119397591-43b2ac80-bcce-11eb-9423-2fcc7aa674ba.png)

## Install and Configure NFS Server
- Run update on the instance `sudo yum -y update`
- Install nfs `sudo yum install nfs*`
- After installation start, enable and check the status of the nfs server service to confirm it is running successfully
```
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```

![image](https://user-images.githubusercontent.com/20463821/119398250-3813b580-bccf-11eb-8f4a-4e47e82c396f.png)

- Setup permission that will allow webservers read, write and execute files on the NFS server

![image](https://user-images.githubusercontent.com/20463821/119398749-e3bd0580-bccf-11eb-9b51-07b08d172c73.png)

- Restart the NFS server service `sudo systemctl restart nfs-server.service`
- Find the subnet CIDR for NFS-Server VM, search and select **Virtual networks** on Azure Portal Dashboard. Click on **Tooling-vnet**

![image](https://user-images.githubusercontent.com/20463821/119401174-0c92ca00-bcd3-11eb-8831-cc8703804e7d.png)

- Click on **Subnets**, the CIDR appears to the right, it is indicated by the red box

![image](https://user-images.githubusercontent.com/20463821/119401519-8034d700-bcd3-11eb-9b81-da456a8b8c09.png)

- Configure access to NFS for clients within the same subnet (Subnet CIDR --> 10.0.0.0/24)
- Open /etc/exports file `sudo vi /etc/exports` and place the following configuration:
```
/mnt/apps 10.0.0.0/24(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 10.0.0.0/24(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 10.0.0.0/24(rw,sync,no_all_squash,no_root_squash)
```
- To save file, press **Esc**, **:wq**
- Run `sudo exportfs -arv` afterwards
- Check the port NFS runs on rpcinfo -p | grep nfs

![image](https://user-images.githubusercontent.com/20463821/119404585-e7ed2100-bcd7-11eb-829d-c9909f26bb03.png)

- In order for NFS server to be accessible from the client, you have to open ports: TCP 111, UDP 111, UDP 2049 and TCP 2049
- Go to **Virtual machines**, select **NFS-Server** and click on **Add inbound port rule**

![image](https://user-images.githubusercontent.com/20463821/119405703-9776c300-bcd9-11eb-86f2-65095eec3f27.png)

![image](https://user-images.githubusercontent.com/20463821/119406452-b88be380-bcda-11eb-91a5-e69a93b0a413.png)

## Configure the database server
- Create a Dbserver virtul machine with Ubuntu 20.04 base image

![image](https://user-images.githubusercontent.com/20463821/119407182-ca21bb00-bcdb-11eb-916d-b45e8b6bd163.png)

- Connect to the virtual machine using ssh `ssh -i <azurekeyfile> <azureusername>@Db-public-ip`
- Run `sudo apt update && sudo apt upgrade`
- Install mysql server on the terminal of the dbserver instance
```
sudo apt install mysql-server
sudo mysql_secure_installation
```
- Login mysql shell `sudo mysql`
- Create database named tooling `CREATE DATABASE tooling;`
- Create a user named webaccess and grant it all privileges on the tooling database
```
CREATE USER '<username>'@'<NFS-Server-private-ip>' IDENTIFIED WITH mysql_native_password BY '<userpassword>';

GRANT ALL PRIVILEGES ON <databasename>.* TO '<username>'@'<NFS-Server-private-ip>' WITH GRANT OPTION;
```
![image](https://user-images.githubusercontent.com/20463821/119409036-98f6ba00-bcde-11eb-8c75-55479438bcad.png)

![image](https://user-images.githubusercontent.com/20463821/119409148-c2afe100-bcde-11eb-8d82-8ae8c401c714.png)

## Prepare the Web Servers
- Launch 3 virtual machines and name them webserver-1, webserver-2 and webserver-3 with RHEL8 os base image

![image](https://user-images.githubusercontent.com/20463821/119411561-854d5280-bce2-11eb-8e70-d58c371b386c.png)

- Connect to the virtual machines using ssh `ssh -i <azurekeyfile> <azureusername>@webserver-public-ip`
- Make update on the instance `sudo yum -y update`
- Install NFS client `sudo yum install nfs-utils nfs4-acl-tools -y`
- Mount /var/www/ and target the NFS server’s export for apps
![image](https://user-images.githubusercontent.com/20463821/119413986-97c98b00-bce6-11eb-9d82-7187be0c782c.png)











