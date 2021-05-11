# DEVOPS TOOLING WEBSITE SOLUTION

![image](https://user-images.githubusercontent.com/20463821/117821748-cc0f6700-b263-11eb-8346-42b46ee4681b.png)

The architecture diagram above depicts the application has 3 webservers that share the same database server while having access to the same files using the Network File System as file storage.

## Spin up NFS Server Instance On AWS Console and Configure LVM on it
- The first step in implementing this project is to prepare the Network File System (NFS) server. Create an EC2 instance with Red Hat Enterprise Linux (RHEL) 8 os base image, named "NFS server"

![image](https://user-images.githubusercontent.com/20463821/117823810-c9ae0c80-b265-11eb-884b-f0116c9f3350.png)

- Create and attach 3 volumes to the NFS server instance from the Elastic Block Store, with size 10GiB and ensure they are in the same availaiblity zone as the NFS server instance

![image](https://user-images.githubusercontent.com/20463821/117824697-991aa280-b266-11eb-862f-682725c86f0f.png)

- Now connect to the instance via ssh `ssh -i keyfile ec2-user@local-ip`
- In the terminal, use `lsblk` to inspect the block devices attached to the server and also the newly created block devices xvdf, xvdg and xvdh
![image](https://user-images.githubusercontent.com/20463821/117825506-4a213d00-b267-11eb-9dad-4ae38989a649.png)

- Create single partition for the 3 disks using gdisk utility `sudo gdisk /dev/xvdf`, `sudo gdisk /dev/xvdg` and `sudo gdisk /dev/xvdh`
![image](https://user-images.githubusercontent.com/20463821/117826857-67a2d680-b268-11eb-8ef5-629d98bb15d2.png)

![image](https://user-images.githubusercontent.com/20463821/117826953-7be6d380-b268-11eb-9d38-c21e60213225.png)

- Use `lsblk` to check the newly configured partitions

![image](https://user-images.githubusercontent.com/20463821/117827335-c8caaa00-b268-11eb-82c9-60994390f284.png)

- Install lvm2 package `sudo yum install lvm2`
- Check the available partitions on the server `sudo lvmdiskscan`

![image](https://user-images.githubusercontent.com/20463821/117827868-40003e00-b269-11eb-8d32-f2fa2c3328b0.png)

- Mark the 3 disks as physical volumes to be used by LVM using `pvcreate`utility
![image](https://user-images.githubusercontent.com/20463821/117830181-4b546900-b26b-11eb-88d2-64c43b30be79.png)

- Verify the physical volumes have been created using `sudo pvs`

![image](https://user-images.githubusercontent.com/20463821/117830347-7343cc80-b26b-11eb-997a-145aefcb2825.png)

- Create a "webserver-vg" volume group, using "vgcreate" utility and sadd the 3 physical volumes to the volume group created.

![image](https://user-images.githubusercontent.com/20463821/117831369-64a9e500-b26c-11eb-8890-ce9bac274d6f.png)

- Verify the Volume group has been created successfully `sudo vgs`

![image](https://user-images.githubusercontent.com/20463821/117831527-8dca7580-b26c-11eb-858e-dc80ab8a8b72.png)

- Create 3 logical volumes namely: `lv-opt`, `lv-apps` and `lv-logs` using "lvcreate" utility. Each lv will take a portion of the physical volume which is 10g, where lv-opt and lv-logs will take 3.3g each and lv-apps will take the rest of the available space which is 3.4g

![image](https://user-images.githubusercontent.com/20463821/117845729-b9535d00-b278-11eb-80ab-b3bc3890cdb1.png)

- Confirm the logical volumes have been created successfully `sudo lvs`

![image](https://user-images.githubusercontent.com/20463821/117845954-e9026500-b278-11eb-85bc-b4f6dd7d258b.png)

- Verify the entire setup `sudo vgdisplay -v`

![image](https://user-images.githubusercontent.com/20463821/117846261-2c5cd380-b279-11eb-88c2-9069b48c7a1f.png)

- Format each of the logical volume with `xfs` file system using `mkfs.xfs` 

![image](https://user-images.githubusercontent.com/20463821/117847178-03890e00-b27a-11eb-8915-b5edab3b98cf.png)

- Create a "/mnt directory", where you can mount the logical volumes created, using `touch` command to create apps, logs and opt files in the mnt directory
![image](https://user-images.githubusercontent.com/20463821/117849747-87dc9080-b27c-11eb-892f-4830ba9574fb.png)

- Mount /mnt/apps on lv-apps logical volume `sudo mount /dev/webserver-vg/lv-apps /mnt/apps` and /mnt/opt on lv-opt logical volume `sudo mount /dev/webserver-vg/lv-opt /mnt/opt`
![image](https://user-images.githubusercontent.com/20463821/117850041-d558fd80-b27c-11eb-8f71-112fd1f1939a.png)

- Backup all files in the log directory "/var/log" into "/mnt/logs" using `rsync` utility `sudo rsync -av /var/log/. /mnt/logs`
![image](https://user-images.githubusercontent.com/20463821/117850432-32ed4a00-b27d-11eb-9704-7d54e310bcda.png)

- Mount /var/log on lv-logs logical volume `sudo mount /dev/webserver-vg/lv-logs /var/log`
- Restore log files in /mnt/logs directory back into /var/log directory `sudo rsync -av /mnt/logs/. /var/log`
![image](https://user-images.githubusercontent.com/20463821/117851220-01c14980-b27e-11eb-9472-08caca411fa2.png)

- Use `sudo blkid` to find the UUID of each device and copy to a notepad

![image](https://user-images.githubusercontent.com/20463821/117851453-4220c780-b27e-11eb-9b83-ad50e2758077.png)

- To make the mount configuration persist after restart, enter the values of the UUID copied in /etc/fstab using `sudo vi /etc/fstab`
![image](https://user-images.githubusercontent.com/20463821/117852578-5d400700-b27f-11eb-81c8-2044ec7a3bfe.png)

- Test the configuration `sudo mount -a` and reload daemon `sudo systemctl daemon-reload`
- Verify setup, ru n `df -h`

![image](https://user-images.githubusercontent.com/20463821/117852877-b5770900-b27f-11eb-9cde-24bebb857bb4.png)

## Install and Configure NFS Server
- Run update on the instance `sudo yum -y update`
- Install nfs `sudo yum install nfs*`
- After installation, start, enable and check the status of the nfs server service to confirm it is running successfully
```
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![image](https://user-images.githubusercontent.com/20463821/117854925-de989900-b281-11eb-8fcd-b7fe080a9778.png)

- Export the mounts for webservers’ subnet cidr to connect as clients
- To check the ec2-instance subnet cidr. In the instance details, click on the subnet link
![image](https://user-images.githubusercontent.com/20463821/117855759-b52c3d00-b282-11eb-8825-e603959af466.png)

![image](https://user-images.githubusercontent.com/20463821/117855966-f3296100-b282-11eb-9033-7b8606971ccc.png)

- Setup permission that will allow webservers read, write and execute files on the NFS server

![image](https://user-images.githubusercontent.com/20463821/117856711-dc373e80-b283-11eb-9ec5-f7afc5d311b1.png)

- Restart the NFS server service `sudo systemctl restart nfs-server.service`
- Configure access to NFS for clients within the same subnet 
