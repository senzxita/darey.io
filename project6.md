# WEB SOLUTION WITH WORDPRESS
- Launch 2 EC2 instances using RedHat image on AWS console

![image](https://user-images.githubusercontent.com/20463821/117064886-aa235b00-ad1e-11eb-8c4e-ce0206792598.png)

- Create 3 volumes from the Elastic Block Store

![image](https://user-images.githubusercontent.com/20463821/117065240-25850c80-ad1f-11eb-8a6a-cca06a55c75e.png)

- Ensure each volume created is allocated 10GiB size and in the same availaibility zone as the webserver instance.
![image](https://user-images.githubusercontent.com/20463821/117065716-bc51c900-ad1f-11eb-8d10-a7226c215a61.png)

- Volume created successfully, repeat steps in creating 2 more volumes.
![image](https://user-images.githubusercontent.com/20463821/117065870-e905e080-ad1f-11eb-89c7-74bf3c949540.png)

- Three volumes have been successfully created

![image](https://user-images.githubusercontent.com/20463821/117066064-2c604f00-ad20-11eb-980d-9b9b55a0531e.png)

- Attach the created volumes to webserver instance, by selecting an instance at a time and select the *Action* dropdown menu and click on "Attach Volume"
![image](https://user-images.githubusercontent.com/20463821/117066384-911ba980-ad20-11eb-9a3d-b66acc921b2d.png)

- Select the instance you want to attach the volume to, in the instance field. Select *webserver* instance and click on the *Attach* button
![image](https://user-images.githubusercontent.com/20463821/117066648-e657bb00-ad20-11eb-8d0b-e28108d214ca.png)

- Successfully attached the 3 volumes to the webserver instance
![image](https://user-images.githubusercontent.com/20463821/117067358-d2608900-ad21-11eb-87a9-f0837d03b2d4.png)

- Connect to both instances created, webserver and dbserver via ssh

Webserver instance

![image](https://user-images.githubusercontent.com/20463821/117067732-4b5fe080-ad22-11eb-869a-adf7dbebe650.png)

Dbserver instance

![image](https://user-images.githubusercontent.com/20463821/117067837-67fc1880-ad22-11eb-9af3-3675cec3cbab.png)

- In webserver instance terminal, use the command `lsblk` to inspect the block devices attached to the server and the newly created block devices are xvdf, xvdg, xvdh

![image](https://user-images.githubusercontent.com/20463821/117069898-f4a7d600-ad24-11eb-93b9-64464788c333.png)

- To see all mounts and free spaces available on the server use `df -h` command

![image](https://user-images.githubusercontent.com/20463821/117070327-7a2b8600-ad25-11eb-9236-79d903c5ff07.png)

- Using `gdisk` utility, create a single partition for each of the 3 disks `sudo gdisk /dev/xvdf`, `sudo gdisk /dev/xvdg` and `sudo gdisk /dev/xvdh` respectively

 ![image](https://user-images.githubusercontent.com/20463821/117073187-7863c180-ad29-11eb-8bb5-77ba279b1903.png)

- Check the newly configured partition on each disk using `lsblk`
 
![image](https://user-images.githubusercontent.com/20463821/117073391-bcef5d00-ad29-11eb-928d-2a8f2974e0d4.png)

- To install `lvm2` package, use `sudo yum install lvm2`
 
![image](https://user-images.githubusercontent.com/20463821/117073582-0770d980-ad2a-11eb-8eb4-f9344bf39487.png)

- To check the available partitions on the server use `sudo lvmdiskscan`
 
![image](https://user-images.githubusercontent.com/20463821/117073762-499a1b00-ad2a-11eb-96bf-0ed68fac2603.png)

- Mark each of the 3 disks as physical volumes to be used by LVM using `pvcreate` utility
 
![image](https://user-images.githubusercontent.com/20463821/117074179-eb216c80-ad2a-11eb-937e-b6eb96759864.png)

- Verify the physical volumes have been created using `sudo pvs`
 
![image](https://user-images.githubusercontent.com/20463821/117074351-173ced80-ad2b-11eb-86eb-e5cfa12c6aca.png)

- Create a volume group named *webdata-vg* using *vgcreate* utility and add the 3 physical volumes to the volume group
 
![image](https://user-images.githubusercontent.com/20463821/117074760-afd36d80-ad2b-11eb-859d-567e6db3279d.png)

- Verify the volume group has been created successfully using `sudo vgs`
 
![image](https://user-images.githubusercontent.com/20463821/117074860-d72a3a80-ad2b-11eb-8419-ccf42b5b47d8.png)

- Create 2 logical volumes using `lvcreate` utility, one of the logical volume named "apps-lv" while the other is named "logs-lv", each will take half the sixe of the physical volume (10g) ie, apps-lv and logs-lv are allocated 5g each

![image](https://user-images.githubusercontent.com/20463821/117076373-325d2c80-ad2e-11eb-922b-e424d7a1e88e.png)

- Verify the logical volumes have been created successfully using `sudo lvs`

![image](https://user-images.githubusercontent.com/20463821/117076484-5de01700-ad2e-11eb-989b-2ae2389bd710.png)

- Verify the entire setup `sudo vgdisplay -v`

![image](https://user-images.githubusercontent.com/20463821/117076650-a1d31c00-ad2e-11eb-8e7f-59a280f6ac16.png)

![image](https://user-images.githubusercontent.com/20463821/117076680-b1526500-ad2e-11eb-8481-ad276adf0d34.png)

- Format the logical volumes created apps-lv and logs-lv, with ext4 file system using `mkfs.ext4`
![image](https://user-images.githubusercontent.com/20463821/117076970-38074200-ad2f-11eb-8e8d-bc4f5d96ea50.png)

- Create /var/www/html dir to store website files `sudo mkdir -p /var/www/html` and create /home/recovery/logs to store backup of log data `sudo mkdir -p /home/recovery/logs`

![image](https://user-images.githubusercontent.com/20463821/117079688-2b391d00-ad34-11eb-8293-a4a23e1760de.png)

- Mount /var/www/html on apps-lv logical volume `sudo mount /dev/webdata-vg/apps-lv /var/www/html`

![image](https://user-images.githubusercontent.com/20463821/117079808-72bfa900-ad34-11eb-9ab1-b8cd65290835.png)

- Backup all files in log directory /var/log into /home/recovery/logs using rsync utility `sudo rsync -av /var/log/. /home/recovery/logs`
![image](https://user-images.githubusercontent.com/20463821/117081217-5ffaa380-ad37-11eb-8c1b-b83224b87988.png)


- Mount /var/log on logs-lv logical volume `sudo mount /dev/webdata-vg/logs-lv /var/log`
- Restore log files in /home/recovery/logs directory back into /var/log directory. `sudo rsync -av /home/recovery/logs/. /var/log

![image](https://user-images.githubusercontent.com/20463821/117081575-0e064d80-ad38-11eb-855e-8372bc1a191e.png)

- Use `sudo blkid` to find the UUID of each device, copy the values to a notepad

![image](https://user-images.githubusercontent.com/20463821/117082312-d1d3ec80-ad39-11eb-974b-1afaabc64a80.png)

- Edit /etc/fstab file so that the mount configuration will persist after restart `sudo vi /etc/stab`, enter the values of the UUID copied earlier and save the file

![image](https://user-images.githubusercontent.com/20463821/117082190-828dbc00-ad39-11eb-84c2-c6acb50d9c55.png)

- Test the configuration and reload the daemon
```
sudo mount -a
sudo systemctl daemon-reload
```
- To verify setup, run `df -h`

![image](https://user-images.githubusercontent.com/20463821/117082705-d947c580-ad3a-11eb-836e-2a79ac7a29e5.png)

## Prepare Database server
- For the dbserver instance on AWS console, create 3 volumes from Elastic Block Store in the same availability zone as the dbserver instance and attach to the dbserver instance

![image](https://user-images.githubusercontent.com/20463821/117083794-6e4bbe00-ad3d-11eb-8c5c-6dc8c73db68b.png)

- On the dbserver instance termninal use `lsblk` to inspect the block devices attached to the server

![image](https://user-images.githubusercontent.com/20463821/117083980-d8fcf980-ad3d-11eb-85cc-d8bc4a557f29.png)

- `df -h` to see mounts and free spaces on the server

![image](https://user-images.githubusercontent.com/20463821/117084061-0f3a7900-ad3e-11eb-81e9-22db453d479c.png)

- Using gdisk utility, create a single partition for each of the 3 disks `sudo gdisk /dev/xvdf`, `sudo gdisk /dev/xvdg` and `sudo gdisk /dev/xvdh` respectively

![image](https://user-images.githubusercontent.com/20463821/117084177-69d3d500-ad3e-11eb-86e3-e868bfd4cf07.png)

- Check the newly configured partition on each disk using `lsblk`

![image](https://user-images.githubusercontent.com/20463821/117084336-d949c480-ad3e-11eb-8242-2de3f95dba75.png)

- Install lvm2 package `sudo yum install lvm2`
- Check available partitions on the server use `sudo lvmdiskscan`
- Mark each of the 3 disks as physical volume to be used as LVM using `pvcreate` utility

![image](https://user-images.githubusercontent.com/20463821/117084579-74db3500-ad3f-11eb-96fa-9d85313bb96d.png)

- Verify the physical volumes using `sudo pvs`
- Create volume group named webdata-vg using vgcreate utility and add the 3 physical volumes to the volume group

![image](https://user-images.githubusercontent.com/20463821/117084836-21b5b200-ad40-11eb-91f0-1b9b2456bcec.png)

- Verify the volume group has been created successfully

![image](https://user-images.githubusercontent.com/20463821/117084876-385c0900-ad40-11eb-8236-9e86d5b21cbe.png)

- Create 2 logical volumes using lvcreate utility 

![image](https://user-images.githubusercontent.com/20463821/117085003-912ba180-ad40-11eb-9857-fc563e9fffcf.png)

- Verify the logical volume has been successfully created `sudo lvs`

![image](https://user-images.githubusercontent.com/20463821/117085088-c2a46d00-ad40-11eb-8888-c13c43171b9f.png)

- Verify the entire setup 

![image](https://user-images.githubusercontent.com/20463821/117085160-f089b180-ad40-11eb-87c7-37d49c4579d6.png)

![image](https://user-images.githubusercontent.com/20463821/117085206-1020da00-ad41-11eb-99dc-dc72b7a0de12.png)

- Format the logical volumes with ext4 filesystem using mkfs.ext4

![image](https://user-images.githubusercontent.com/20463821/117085720-6d695b00-ad42-11eb-8633-a2bad622439e.png)

- Create /db directory to store the database `sudo mkdir -p /db`
- Create /home/recovery/logs to store backup of log data `sudo mkdir -p /hoime/recovery/logs`
- Mount /db on dv-lv logical volume `sudo mount /dev/webdata-vg/db-lv /db`
- Use rsync utility to backup all files in log directory /var/log into /home/recovery/logs

- ![image](https://user-images.githubusercontent.com/20463821/117086029-5bd48300-ad43-11eb-8479-c1f4bba91b69.png)

- Mount /var/log on logs-lv logical volume 



 













