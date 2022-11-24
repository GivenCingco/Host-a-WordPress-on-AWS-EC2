# WordPress website hosted on an Amazon EC2 instance

You must have an existing AWS account. Every resource we use will be under free tier. ***Free tier: In your first year includes 750 hours of t2.micro (or t3.micro in the Regions in which t2.micro is unavailable) instance usage on free tier AMIs per month, 30 GiB of EBS storage, 2 million IOs, 1 GB of snapshots, and 100 GB of bandwidth to the internet***.
- Sign into the AWS management console and ensure that you are in the N.Virginia(us-east-1) region.
- Go to the EC2 [dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#LaunchInstances:) and click the orange 'Launch Instance' button.

![Screenshot 2022-11-22 at 15 22 43](https://user-images.githubusercontent.com/50238769/203331652-41a764ab-3ebf-45a6-a9e4-2dbd1997f9aa.png)


- Select the Ubuntu SERVER 22.04 lts (hvm), ssd Volume Type AMI.



![Screenshot 2022-11-22 at 15 37 44](https://user-images.githubusercontent.com/50238769/203333206-f9a5e284-dcd5-4d32-bb73-904f3b612aa4.png)
  - Give the server a name. 
  - Select the t2.micro istance type (free tier eligible).
  - Create a new key pair:
      -Name your keypair.
      - Type and format must be RSA and .pem file format.

- In the Network settings section create security group inbound rules. Security groups are stateful, meaning any changes applied to an incoming rule will be automatically applied to the outgoing rule.
  - SSH - Allow SSH traffic from from anywhere(0.0.0.0/0). 
  - HTTPS - Allow HTTPS traffic from anywhere(0.0.0.0/0).
  - HTTP - Allow HTTP traffic from anywhere (0.0.0.0/0).
- Configure Storage to be 20 GiB.
- Continue through the setup keeping the defaults launch the instance.


#Login to your EC2 via SSh
- Go to downloads where your key pair is stored.
- Create a folder and call it 'SSH'. 
- Copy the key pair and paste it inside the newly created SSH folder. 
- Open your terminal type the following commands in the screenshot below to change into the directory where your key pair is stored. 

![Screenshot 2022-11-23 at 14 38 47](https://user-images.githubusercontent.com/50238769/203549430-657fab69-27ab-4592-bf41-c935793d8ea9.png)


- Select the your EC2, under the details tab select your Public IPv4 address.

![Screenshot 2022-11-23 at 14 44 38](https://user-images.githubusercontent.com/50238769/203550377-731fd2c9-439a-4fc3-ab78-c141d479eec2.png)

## SSh into instance via terminal
- Go back to your terminal to login to your EC2 via SSH
- Execute these commands below:
  - Type '***chmod 400[Key pair name]***' and press enter. 
  - Type '***ssh ubuntu@[Public IPv4 address] -i [Key pair name]***' and press enter. Type yes when prompted if you want to continue connecting. 
  - Type '***sudo su -***' to have root priviledges. 

## Update and upgrade LAMP packages. 
Execute the following commands:
  - '***sudo apt update -y*** and press enter. 
  - '***sudo apt upgrade -*** and press enter. Type yes when prompted if your want to continue.
  - If the pink error message appears, simply press okay and escape. Because of the updates we just installed, that error wants us to reboot, but we don't want to reboot, so we pressed escape.
  <img width="1440" alt="Screenshot 2022-11-23 at 15 43 53 (2)" src="https://user-images.githubusercontent.com/50238769/203562140-1f42e80b-461f-4107-94d3-df72dfe329e5.png">
  
  # Setup the LAMP server  (this will enable us to run PHP on the server)
  
  ## Install apache2
  - Type '***apt install apache2 -y***'
  - If the pink error message appears, simply press okay and escape. Because of the updates we just installed, that error wants us to reboot, but we don't want to reboot, so we pressed escape.
  ## Check status if Apache2 is running
  -Type '***systemctl status apache2***' and press enter. 
  
  
  ![Screenshot 2022-11-23 at 15 49 40](https://user-images.githubusercontent.com/50238769/203563438-cf5abe67-b83d-4929-88d8-9257567e0bd1.png)
  
  
  - If apache2 is not running type '***systemctl start apache2***'
  - Copy your EC2 Public IPv4 address and paste it on your browser, Ubuntu apache2 default page should appear.
  
  ![Screenshot 2022-11-23 at 15 56 51](https://user-images.githubusercontent.com/50238769/203565041-eb98e081-bd5a-4ed3-815f-2168fcc4a324.png)
  
  ## Debugging If the page doesn't appear
  - Check your security groups( HTTP/ HTTPS inbound rules). 
  - Check if apache2 is running, if it's not not restart the service. 

- Type '***systemctl enable apache2***' to enable the service so that you dont have to occassionally enable the EC2 machine manually, this is because the service occassionally stops. 


# Install MariaDB
-Type the following commands to install MariaDB
  -'***sudo apt install mariadb-server mariadb-client -y***'
  - '***sudo systemctl start mariadb***' to start the service. 
  - '***sudo systemctl status mariadb***' to check if the service is running. 

# Setup root password of the database
- Secure your MariaDB installation by typing the command '***mysql_secure_installation***'. When prompted, answer Y.
  - It will ask you a few questions and you have to give it answers/confirmations (press enter for no answers).
    -  Enter current password for root (enter for none):Enter 
    -  Switch to unix_socket authentication [Y/n]: Enter
    -  Change the root password[y/n]: Y
    -  New password: [provide your password]
    -  Re-enter password: [provide the same password]
    -  Remove anonymous users [Y/n]: Y (we will create our own)
    -  Disallow root login remotely[Y/n]: Y
    -  Remove test database and access to it [y/n]: Y
    -  Reload privileges table now [y/n]: Y
    
![Screenshot 2022-11-23 at 16 14 19](https://user-images.githubusercontent.com/50238769/203568956-4451d65b-6473-456f-b1d6-b85150073943.png)

- To ensure that all the configurations are saved type the command '***systemctl restart mariadb***'

# Install PHP on the server

Type these commands
- '***apt install php php-mysql php-gd php-cli php-common -y***' to install php

Download WordPress from [https://wordpress.org/download/](https://wordpress.org/download/)

- Go to the button ‘download Wordpress’ right click on it to copy the link address

  ![Screenshot 2022-11-23 at 16 35 24](https://user-images.githubusercontent.com/50238769/203573668-da9bb24c-3aca-4205-b0fa-bdb6b3b33f08.png)
- Type this commands '***apt install wget unzip -y***' - we will use these packages to download PHP from the link address we copied, and unzip the zipped file we will download.
- Download WordPress from the link - Type the command '***wget https://wordpress.org/latest.zip**'
- Type command '***ls***' to list file in the directory

![Screenshot 2022-11-23 at 16 42 16](https://user-images.githubusercontent.com/50238769/203575010-2dde2c23-57c0-4b24-a4aa-b16936acb61a.png)

- Type command '***unzip latest.zip***' to unzip the WordPress zip file we downloaded. 
- Type command '***ls***' WordPress folder should appear. 
- To list all the content type the command '***cd wordpress/***'
- Type the following commands to copy all the content to the ***/var/www/html/*** directory
  - ***cd wordpress/***
  - ***cd ..*** 
  - ***cp -r wordpress/* /var/www/html***
  - ***cd /var/www/html***
  - ***ls -l***
  
![Screenshot 2022-11-23 at 17 00 53](https://user-images.githubusercontent.com/50238769/203579033-9a1df57b-d85c-43c9-a1ec-7a4d979b6455.png)

Looking at the screenshot above you can see that all the permissions are for the root user. 
- Apache server is run by data user, so we have to change the  ownership from root to data user so that we can access the pages. 
- To change the permissions type the command '*** chown www-data:www-data -R /var/www/html/***'

![Screenshot 2022-11-23 at 17 06 39](https://user-images.githubusercontent.com/50238769/203580254-3507273c-f07a-4773-91c0-ecffdcef6171.png)
- From the screenshot above you can see the permissions have changed from root to www-data (data user). 
- Remove the index.html page of the apache server, because when you browse the page (EC2 IP address) you will still see the apache default page which is still rendering  Type the command '***- rm  -rf index.html***'  
- Refresh your browser and you will now see the WordPress setup page. Select your preferred language ans click the continue button. 
- To connect to the database to establish connection and setup database credentials
   - Type the following commands:
      - '***mysql u- root -p***'.
      - Enter password (Enter password that we previously set when setting up MariaDB)
      - '***create database wordpress;*** to create database. 
      - To create user type command '***"wpadmin" identified by "wpadminpass";***
      - To grant access for the user we just created to all the objects of the WordPress database we created, type the command '***grant all privileges on wordpress.* to "wpadmin"***';

![install-step3_v47](https://user-images.githubusercontent.com/50238769/203584444-bfc44cdc-6944-4d6b-b910-8d0c4fca2056.png)

- Enter the correct databse details on the page like on the screenshot above. 
- Click on 'Run the installation' and the browser will take you to a page that looks like the screenshot below.  
![zxxIj](https://user-images.githubusercontent.com/50238769/203585997-ab7a8ff7-4666-49a2-976e-1aa9100c5b18.png)

- Enter all the required fields and click the 'Install WordPress' button'.
- You will be taken to a login page where you will enter your credentials. If the login is successful you should be taken to your WordPress admin panel.

##NB
I accidentally left SSH open to all IP addresses and had to delete the rule in my security group. It is not a good security practice to leave this open because anyone in the world could gain access to your EC2 Instance and all of its data.

