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

- Mount /var/www/html on apps-lv logical volume `sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

![image](https://user-images.githubusercontent.com/20463821/117079808-72bfa900-ad34-11eb-9ab1-b8cd65290835.png)














