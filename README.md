# Project 7
Devops Tooling Website Solution
## Prepare NFS Server
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


STEP 2 — CONFIGURE THE DATABASE SERVER

![Screen Shot 2022-05-31 at 11 53 22 AM](https://user-images.githubusercontent.com/101080172/171320120-55e68522-fbe6-44a9-95d1-89a7c398f7a5.png)

![Screen Shot 2022-05-31 at 12 10 21 PM](https://user-images.githubusercontent.com/101080172/171320248-360ff34f-6399-410d-a2e8-7a52206e0e43.png)
