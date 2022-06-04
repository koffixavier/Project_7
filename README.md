# Project 7
## Devops Tooling Website Solution

As a member of a DevOps team, I will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.


## STEP 1: PREPARED NFS SERVER
- Configured LVM on the Server

![Screen Shot 2022-05-31 at 11 15 26 AM](https://user-images.githubusercontent.com/101080172/171314926-934ffcce-e03b-4dd2-83d6-0ef48de3eb04.png)

-------------------------------------------------------------------------------------


![Screen Shot 2022-05-31 at 11 16 07 AM](https://user-images.githubusercontent.com/101080172/171314931-674e15ea-825c-43aa-8826-ddd88b4eb175.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 10 25 05 PM](https://user-images.githubusercontent.com/101080172/171315450-94cf8292-ca6b-412c-9497-79cbdcc16562.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 19 25 AM](https://user-images.githubusercontent.com/101080172/171318915-f9f41f0d-0bd7-43ef-b4f7-35dcf92ad6de.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 22 06 AM](https://user-images.githubusercontent.com/101080172/171319131-cbd865cb-8192-4ce9-824b-8e74e8b3ca93.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 25 12 AM](https://user-images.githubusercontent.com/101080172/171319172-c5693f20-a29b-4025-a424-547a39c570c1.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 30 33 AM](https://user-images.githubusercontent.com/101080172/171319315-b084abc2-c8e7-46a9-8a55-6f2e2442c00a.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 30 53 AM](https://user-images.githubusercontent.com/101080172/171319396-fed71349-f3a7-49c0-bcb5-fb9ce464cf8f.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 31 24 AM](https://user-images.githubusercontent.com/101080172/171319426-9fa1588d-c22c-42fd-84d1-e46ed473fef4.png)

-------------------------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 34 48 AM](https://user-images.githubusercontent.com/101080172/171319489-39303409-7d68-4178-8595-e55a7910bbfb.png)

--------------------------------------------------------------------

![Screen Shot 2022-05-31 at 11 47 07 AM](https://user-images.githubusercontent.com/101080172/171319635-c122f084-d904-4105-8661-46e062a63502.png)

-------------------------------------------------------------------------------------

- Installed NFS server, configured it to start on reboot and make sure it is updated and running

- Command used are listed below 
  ````
   sudo yum -y update
   sudo yum install nfs-utils -y
   sudo systemctl start nfs-server.service
   sudo systemctl enable nfs-server.service
   sudo systemctl status nfs-server.service
  ````


![Screen Shot 2022-05-31 at 11 48 57 AM](https://user-images.githubusercontent.com/101080172/171320638-5c97cc84-3776-4e15-befd-83a0f9a65afa.png)


![Screen Shot 2022-05-31 at 12 12 16 PM](https://user-images.githubusercontent.com/101080172/171320283-9bc9bddc-4504-48c8-abde-ffd80567fade.png)

![Screen Shot 2022-05-31 at 12 14 32 PM](https://user-images.githubusercontent.com/101080172/171320706-9be8ef46-0e71-4e9e-973e-2f9669fbe8bd.png)

-------------------------------------------------------------------------------------

- Export the mounts for webservers’ subnet cidr to connect as clients

  - we set up permission that will allow our Web servers to read, write and execute files on NFS:

  
 ````
  sudo chown -R nobody: /mnt/apps
  sudo chown -R nobody: /mnt/logs
  sudo chown -R nobody: /mnt/opt

  sudo chmod -R 777 /mnt/apps
  sudo chmod -R 777 /mnt/logs
  sudo chmod -R 777 /mnt/opt
````

- We resated the server using `sudo systemctl restart nfs-server.service`

![Screen Shot 2022-05-31 at 12 18 32 PM](https://user-images.githubusercontent.com/101080172/171444203-07e49b78-9d72-4a4d-b3d0-aad572725ece.png)

- Configured access to NFS for clients within the same subnet ( the content of your configuration is shown using the `cat' command)

![Screen Shot 2022-05-31 at 12 26 06 PM](https://user-images.githubusercontent.com/101080172/171444233-b6bbb677-c1a5-429f-b08e-35dac729a1e8.png)


![Screen Shot 2022-05-31 at 12 27 17 PM](https://user-images.githubusercontent.com/101080172/171444265-2d86e72b-2bf5-42d1-88b3-08b074880a29.png)
-------------------------------------------------------------------------------------


- Checked which port is used by NFS and open it using Security Groups `rpcinfo -p | grep nfs`

![Screen Shot 2022-05-31 at 12 27 59 PM](https://user-images.githubusercontent.com/101080172/171453413-0beb93d3-b76e-40f8-b9b2-5d8803a01198.png)


## STEP 2: CONFIGURE THE DATABASE SERVER

![Screen Shot 2022-05-31 at 11 53 22 AM](https://user-images.githubusercontent.com/101080172/171320120-55e68522-fbe6-44a9-95d1-89a7c398f7a5.png)

![Screen Shot 2022-05-31 at 12 10 21 PM](https://user-images.githubusercontent.com/101080172/171320248-360ff34f-6399-410d-a2e8-7a52206e0e43.png)

## Step 3: PREPARE THE WEBSERVER 
- Launched Three new EC2 instances with RHEL 8 Operating System

(The following steps are repeated twice) 

- Installed NFS client
![Screen Shot 2022-05-31 at 12 37 43 PM](https://user-images.githubusercontent.com/101080172/171777780-2f815d2c-ae76-44ae-8113-933d3a27b67e.png)


- Mount /var/www/ and target the NFS server’s export for apps

![Screen Shot 2022-05-31 at 12 42 57 PM](https://user-images.githubusercontent.com/101080172/171778003-6c3221d0-f626-4496-9822-bfb0f1893dcf.png)

- Verified that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot
- added the following line to /etc/fstab

`<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0`

![Screen Shot 2022-05-31 at 12 46 33 PM](https://user-images.githubusercontent.com/101080172/171778138-4b60cf0e-a115-45ce-97f9-ad583354da53.png)

- Install Remi’s repository, Apache and PHP
```
sudo yum install httpd -y

sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module reset php

sudo dnf module enable php:remi-7.4

sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

setsebool -P httpd_execmem 1
```

![Screen Shot 2022-05-31 at 12 47 43 PM](https://user-images.githubusercontent.com/101080172/171778567-9c5737e1-700d-4586-816d-e352f8f72597.png)

![Screen Shot 2022-05-31 at 12 48 51 PM](https://user-images.githubusercontent.com/101080172/171780505-745b4277-855e-4f9a-acb4-0dd8b010bcc0.png)


![Screen Shot 2022-05-31 at 12 49 57 PM](https://user-images.githubusercontent.com/101080172/171780561-8899ead2-405c-49a4-992f-173fcb56acf9.png)

![Screen Shot 2022-05-31 at 12 50 51 PM](https://user-images.githubusercontent.com/101080172/171780579-9a6ba43c-3102-4126-a872-ec294b5255be.png)

![Screen Shot 2022-05-31 at 12 51 28 PM](https://user-images.githubusercontent.com/101080172/171780607-cda69735-a56a-49c6-88b8-eda0835578c5.png)

![Screen Shot 2022-05-31 at 12 52 09 PM](https://user-images.githubusercontent.com/101080172/171780737-48435287-1e66-4321-b444-c6e0369e21e7.png)

![Screen Shot 2022-05-31 at 12 53 58 PM](https://user-images.githubusercontent.com/101080172/171780905-836f9542-0d41-4876-8885-da15ce1b7e63.png)

![Screen Shot 2022-05-31 at 1 19 34 PM](https://user-images.githubusercontent.com/101080172/171781056-d4ea03ea-c1fb-4cc1-a4dd-6b47e97f95e5.png)

- Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. If you see the same files – it means NFS is mounted correctly. You can try to create a new file touch test.txt from one server and check if the same file is accessible from other Web Servers.

- Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №4 to make sure the mount point will persist after reboot.

- Fork the tooling source code from Darey.io Github Account to your Github account. (Learn how to fork a repo here)

- Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html



- Note 2: If you encounter 403 Error – check permissions to your /var/www/html folder and also disable SELinux sudo setenforce 0
To make this change permanent – open following config file sudo vi /etc/sysconfig/selinux and set SELINUX=disabledthen restrt httpd.



- Update the website’s configuration to connect to the database (in /var/www/html/functions.php file). Apply tooling-db.sql script to your database using this command mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql

- Create in MySQL a new admin user with username: myuser and password: password:

- INSERT INTO ‘users’ (‘id’, ‘username’, ‘password’, ’email’, ‘user_type’, ‘status’) VALUES
-> (1, ‘myuser’, ‘5f4dcc3b5aa765d61d8327deb882cf99’, ‘user@mail.com’, ‘admin’, ‘1’);

- Open the website in your browser http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php and make sure you can login into the websute with myuser user.

![Screen Shot 2022-05-31 at 1 29 33 PM](https://user-images.githubusercontent.com/101080172/171781228-f28c0f1c-9bbb-4b47-9a4e-b8f48b705a93.png)


![Screen Shot 2022-05-31 at 1 40 23 PM](https://user-images.githubusercontent.com/101080172/171781418-bb42e78a-4cfd-49fa-81e5-ea11ce9894dd.png)

![Screen Shot 2022-05-31 at 1 46 07 PM](https://user-images.githubusercontent.com/101080172/171781645-30187a38-a3e1-4532-b3fd-c20bce997719.png)

![Screen Shot 2022-05-31 at 1 55 54 PM](https://user-images.githubusercontent.com/101080172/171781722-a1e96473-ee70-4332-9a7d-e4b0d707b020.png)
  
![Screen Shot 2022-05-31 at 4 41 42 PM](https://user-images.githubusercontent.com/101080172/171939078-48b13af7-90cf-4f90-805c-132adfb69cbb.png)


![Screen Shot 2022-05-31 at 2 00 38 PM](https://user-images.githubusercontent.com/101080172/171781767-4ed3c0fd-1920-47ae-aae3-bb7384640df9.png)


![Screen Shot 2022-05-31 at 2 41 56 PM](https://user-images.githubusercontent.com/101080172/171781750-a05b60ca-86c7-42ae-a5ab-e27a42620c35.png)

![Screen Shot 2022-05-31 at 4 40 57 PM](https://user-images.githubusercontent.com/101080172/171782245-d61c941b-41a0-4a21-80fd-5a76d8646444.png)
  
![Screen Shot 2022-05-31 at 4 41 05 PM](https://user-images.githubusercontent.com/101080172/171782264-148e1447-278b-470c-931f-a7d6034f8357.png)
  
- Some of the history command for one of my Webserver
  
![Screen Shot 2022-06-03 at 11 13 39 PM](https://user-images.githubusercontent.com/101080172/171978223-a293b6dc-36e2-478c-b57f-c28276f3d709.png)
![Screen Shot 2022-06-03 at 11 13 54 PM](https://user-images.githubusercontent.com/101080172/171978242-c5de88a9-da6a-4575-8037-58c4a2311957.png)

