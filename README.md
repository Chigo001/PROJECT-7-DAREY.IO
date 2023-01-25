PROJECT 7

Devops Tooling Website Solution

STEP 1

PREPARE NFS SERVER
- To begin, I spinned up a new EC2 instance with RHEL Linux 9 Operating Systemas it was the available free tier at the time.
<img width="1440" alt="Screenshot 2023-01-18 at 2 34 33 PM" src="https://user-images.githubusercontent.com/115854902/214278910-976d6378-b1eb-4c3d-9d07-6f9ca7a7f8db.png">
- Next, was to configure LVM on the server. I created 3 logical volumes and attached them to the NFS Server.
<img width="1440" alt="Screenshot 2023-01-18 at 2 33 43 PM" src="https://user-images.githubusercontent.com/115854902/214279016-c68f0168-d55f-445c-b523-d3a69f3c7481.png">
- I used the lsblk command to ensure that the disk has been attached to the server.
- I created single partitions on each of the 3 disks using the gdisk utility.

<img width="1440" alt="Screenshot 2023-01-19 at 1 40 53 PM" src="https://user-images.githubusercontent.com/115854902/214281061-25f5e513-fa9f-41f3-8262-89fbc09bfac9.png">
<img width="1440" alt="Screenshot 2023-01-19 at 1 41 55 PM" src="https://user-images.githubusercontent.com/115854902/214281124-f93bc6ef-b5ff-4223-84a8-9e68394ebe1a.png">
<img width="1440" alt="Screenshot 2023-01-19 at 1 42 48 PM" src="https://user-images.githubusercontent.com/115854902/214281519-aa687aa8-c274-496b-a6cd-9c086799b01a.png">
- I used the lsblk command to view and confirm my newly configured partition on each of the 3 disk.
<img width="1440" alt="Screenshot 2023-01-19 at 1 43 50 PM" src="https://user-images.githubusercontent.com/115854902/214282248-4e722f0c-64ce-4f2a-ba95-8189cc72b33b.png">

- The next step was to install the lvm2 package using 'sudo yum install lvm2'. I also checked for available partitions using the command 'sudo lvmdiskscan'.
<img width="1440" alt="Screenshot 2023-01-19 at 1 45 21 PM" src="https://user-images.githubusercontent.com/115854902/214282516-ac6e1072-2de0-452f-9e6f-7e5b4c4f5ec0.png">
- I went ahead to use the pvcreate utility to mark each of the 3disks as physical volumes to be used by the LVM and also verified that the physical volumes have been successfully created with the command 'sudo pvs'.
- I added all 3 PVS to a volume group using the vgcreate utility and named it "webdata-vg". I also verified using the sudo vgs command.

<img width="1440" alt="Screenshot 2023-01-19 at 1 56 17 PM" src="https://user-images.githubusercontent.com/115854902/214284062-21e72469-71f0-4c86-9869-3043f53aace1.png">

- Next, was to create 3 logical volumes named lv-apps, lv-logs, lv-opt. I also verified that the logical volumes have been successfully created and also the entire set up.
<img width="1440" alt="Screenshot 2023-01-19 at 2 03 23 PM" src="https://user-images.githubusercontent.com/115854902/214285408-03c50a80-ef8e-4cec-aa53-59d593e572d8.png">
- I used mkfs.xfs to format the logical volumes.
<img width="1440" alt="Screenshot 2023-01-19 at 2 10 07 PM" src="https://user-images.githubusercontent.com/115854902/214286816-55eba22d-d9ac-4528-a5d1-74af2041e7f1.png">
- I created mount points on /mnt directory for the logical volumes.
<img width="1440" alt="Screenshot 2023-01-19 at 2 24 42 PM" src="https://user-images.githubusercontent.com/115854902/214287771-a9a9da36-84d7-4f28-9b37-4bdec1493084.png">
- I installed and configured the NFS server to start on reboot and ensure it was up and running.
- Next, was to set up permission that will allow our web servers to read, write and execute files on NFS.
- I configured access to NFS for clients within the same subnet.
- I went ahead to check which port is used by NFS and opened it using security groups.

<img width="1440" alt="Screenshot 2023-01-19 at 2 26 39 PM" src="https://user-images.githubusercontent.com/115854902/214289129-2a58c1aa-ac07-4dd8-afe2-bae06c512ea8.png">
<img width="1440" alt="Screenshot 2023-01-19 at 2 27 52 PM" src="https://user-images.githubusercontent.com/115854902/214289179-60249cd2-c3fb-4bdf-82c8-1c77e279d521.png">
<img width="1440" alt="Screenshot 2023-01-19 at 2 30 03 PM" src="https://user-images.githubusercontent.com/115854902/214289238-cb5411c2-8ae6-4f37-8cbc-9c41f3ad92cd.png">
<img width="1440" alt="Screenshot 2023-01-19 at 5 56 44 PM" src="https://user-images.githubusercontent.com/115854902/214289375-ea7da55a-5cb4-4f89-9aa6-6b69d0622de0.png">
<img width="1440" alt="Screenshot 2023-01-20 at 10 50 08 AM" src="https://user-images.githubusercontent.com/115854902/214293157-6fd7d1d9-8ecc-4fd6-8bd9-29c0615cc8b6.png">

STEP 2

CONFIGURE THE DATABASE SERVER 

- I first and foremost updated and installed mysql server.
- Next, was to create a database and name it tooling.
- Then, i created a database user and named it webaccess.
- Also, granted permission to webaccess user on tooling database to do anything only from the webservers subnet cidr.
<img width="1440" alt="Screenshot 2023-01-20 at 11 12 00 AM" src="https://user-images.githubusercontent.com/115854902/214295480-3ab4e892-20ab-44c0-8f8c-3113ce6f3e58.png">
<img width="1440" alt="Screenshot 2023-01-20 at 11 15 03 AM" src="https://user-images.githubusercontent.com/115854902/214295586-9f147992-c511-43f7-a5aa-94ec30bf1ecf.png">
<img width="1440" alt="Screenshot 2023-01-20 at 11 28 22 AM" src="https://user-images.githubusercontent.com/115854902/214295642-f31638cc-6dc1-4bbb-bbe8-64427e759adc.png">
<img width="1440" alt="Screenshot 2023-01-20 at 5 02 29 PM" src="https://user-images.githubusercontent.com/115854902/214295853-a7eeffb6-9bc1-4d11-bd1a-241b131b7d25.png">

STEP 3

PREPARE THE WEB SERVERS
- To begin, i spinned up a new EC2 instance.
- Then installed NFS client.
<img width="1440" alt="Screenshot 2023-01-20 at 11 35 47 AM" src="https://user-images.githubusercontent.com/115854902/214297661-dd121973-1330-4ed3-8cc6-1a621fac86d1.png">
I mounted /var/www and target the NFS server's export for apps.
- I verified the NFS was mounted successfully, then made sure that the changes will persist on the web server after reboot.
<img width="1440" alt="Screenshot 2023-01-20 at 11 41 22 AM" src="https://user-images.githubusercontent.com/115854902/214304315-f66a9096-9524-4f7b-b74e-6ad94357ef19.png">
<img width="1440" alt="Screenshot 2023-01-20 at 12 30 39 PM" src="https://user-images.githubusercontent.com/115854902/214304336-f92ed340-bbfc-45f0-818e-0a2f1b0f921c.png">

I also installed Remi's repository, Apache and PHP.
<img width="1440" alt="Screenshot 2023-01-20 at 12 56 07 PM" src="https://user-images.githubusercontent.com/115854902/214305738-01e8fb48-0b07-4aae-a87b-0170e5b22c68.png">
<img width="1440" alt="Screenshot 2023-01-20 at 12 57 15 PM" src="https://user-images.githubusercontent.com/115854902/214305761-f8443f7c-cf8a-4ccb-b99b-792694bd5f54.png">
<img width="1440" alt="Screenshot 2023-01-20 at 12 58 08 PM" src="https://user-images.githubusercontent.com/115854902/214305793-344404e3-9b63-4771-89c4-8c538de91101.png">
<img width="1440" alt="Screenshot 2023-01-20 at 1 04 12 PM" src="https://user-images.githubusercontent.com/115854902/214306584-a0014e26-952d-4a94-b307-3ee6779864c8.png">
<img width="1440" alt="Screenshot 2023-01-20 at 1 30 02 PM" src="https://user-images.githubusercontent.com/115854902/214306925-680fe034-fe62-4ad0-8094-d1dcfff7b2d5.png">

- I repeated all steps for the 2 other web servers.
- I verified that Apache files and directions are available on the webserver in /var/www and also on the NFS server in /mnt/apps and i saw same files.
<img width="1440" alt="Screenshot 2023-01-20 at 1 16 32 PM" src="https://user-images.githubusercontent.com/115854902/214308271-0fb04f2f-cc46-41ef-b6fa-6dfeef6c5634.png">
<img width="1440" alt="Screenshot 2023-01-20 at 1 16 43 PM" src="https://user-images.githubusercontent.com/115854902/214308276-01a05d0d-2dda-4be0-95e9-1024027eb125.png">
- I located the log folder for apache on the sever and mounted it to NFS server export for logs. I also opened the /etc/fstab file and added the mount point to ensure it persist after reboot.
<img width="1440" alt="Screenshot 2023-01-20 at 1 30 02 PM" src="https://user-images.githubusercontent.com/115854902/214308980-9d7088a4-9425-41af-8f3e-27b6b7b1da6d.png">
- I forked the tooling source code from Darey.io Github account to my github account. I installed git then cloned the source code.
<img width="1440" alt="Screenshot 2023-01-20 at 1 34 42 PM" src="https://user-images.githubusercontent.com/115854902/214310102-73a53726-470b-43c4-8a78-2268724c6de9.png">
<img width="1440" alt="Screenshot 2023-01-20 at 1 35 43 PM" src="https://user-images.githubusercontent.com/115854902/214310121-32591e79-029f-4afe-9bac-3da5251126f0.png">
- I deployed the tooling website's code to the webserver and ensured that the html folder from the repository is deployed to /var/www/html. 
- I also opened TCP port 80 on the webserver.
- I checked permissions to /var/www/html folder and also disabled selinux.
<img width="1440" alt="Screenshot 2023-01-20 at 1 53 58 PM" src="https://user-images.githubusercontent.com/115854902/214311094-a97f6bd3-06ca-4dda-b651-d804f8c16442.png">

- I went ahead to update the websites configuration to connect to the database and applied tooling to the database.
- I created a new admin user in my SQL.
- I opened the website in my browser.

<img width="1440" alt="Screenshot 2023-01-20 at 4 37 47 PM" src="https://user-images.githubusercontent.com/115854902/214312786-6f6f3f80-d435-4bd2-b78e-6c0cf2ad63d1.png">
<img width="1440" alt="Screenshot 2023-01-20 at 1 57 03 PM" src="https://user-images.githubusercontent.com/115854902/214312818-02226c1e-d386-4d19-a014-a036b858b771.png">

<img width="1440" alt="Screenshot 2023-01-20 at 5 01 42 PM" src="https://user-images.githubusercontent.com/115854902/214312871-88a75b62-c324-410d-8e81-38de91318e5b.png">

<img width="1440" alt="Screenshot 2023-01-20 at 5 01 54 PM" src="https://user-images.githubusercontent.com/115854902/214312907-6e99b94b-2ccc-4dbe-bcd5-11daaec19a91.png">




















