![WordPress drawio](https://user-images.githubusercontent.com/50238769/204006061-449d0a85-f2ee-4f89-ac1c-2281d2ddac87.png)



# AWS Amazon Linux EC2: Hosting WordPress website free for one year
### NB: Everything was done on a Mac, so other steps may differ if you use a different operating system.

## What is WordPress?
WordPress is widely chosen as a CMS because of its user-friendliness, adaptability and an extensive community. Its extensive collection of plugins and themes, search engine optimization-friendly characteristics and the ability to handle large traffic make it suitable for various types of websites. Being open-source, free to use and able to be self-hosted makes it a cost-efficient solution. It can be used to create a wide range of websites, including blogs, e-commerce sites, and portfolios, and can be easily customized using a variety of themes and plugins.


## Module 1 : Relational Database

Amazon RDS for MySQL is a cost-efficient and secure solution for hosting a MySQL database for a WordPress website. It provides scalability, high availability, security, and ease of management. The pay-as-you-go pricing model allows for cost-efficiency and eliminates the need for upfront costs or ongoing maintenance for a physical infrastructure. Additionally, it is compatible with WordPress, which uses MySQL as its database management system and it integrates seamlessly with other AWS services such as Elastic Beanstalk, CloudFront, and Lambda for easy deployment, caching and event-driven processing.

- Access the AWS Management Console and navigate to the ***[RDS](https://us-east-1.console.aws.amazon.com/rds/home?region=us-east-1#)*** section.
- Choose the ***Create database*** button to get started.


![Screenshot 2023-01-17 at 08 34 49](https://user-images.githubusercontent.com/50238769/214060549-3b289fe6-ddd1-4b54-9b8a-a9b47cc2b5bf.png)

- WordPress uses MySQL, so select ***Standard create*** for the database creation method and choose the ***MySQL*** engine. 

![Screenshot 2023-01-17 at 08 39 03](https://user-images.githubusercontent.com/50238769/214060671-73c5051a-dc72-4a56-9362-4921938ae8ea.png)

- Make sure to choose the ***"Free tier"*** option under ***"Templates"*** to avoid any charges. If ***"Production"*** or ***"Dev/Test"*** were selected, the options for availability and durability would not be disabled.

![Screenshot 2023-01-17 at 08 42 19](https://user-images.githubusercontent.com/50238769/214060903-5cfa564d-622f-4108-b21e-36a5655f1209.png)

- In the ***Settings*** section, enter wordpress as your ***DB instance identifier***. Then specify the master username and password for your database. Choose a strong, secure password to help protect your database. Store the username and password for safekeeping as you will need it in a later module.

![Screenshot 2023-01-17 at 09 13 40](https://user-images.githubusercontent.com/50238769/214061067-624a1975-dea3-41a9-853f-51a42c56ae5f.png)

- The default settings for instance configuration, storage, connectivity, and database authentication will suffice for the following sections in this guide.
- Finally, Amazon RDS provides a number of additional configuration options to customise your deployment. You need to make one change in this area. Select the Additional configuration line to expand the options.

Set the Initial database name to wordpress. This will ensure Amazon RDS creates the database in your MySQL instance upon initialisation. You will use this database name when connecting to your database. 
- To create your database, click on the orange ***'Create database'*** button.

![Screenshot 2023-01-17 at 09 27 59](https://user-images.githubusercontent.com/50238769/214061380-ee384f4a-dbd1-45fc-ba2b-4f172d7fd68e.png)

# Module 2 : Creating an Elastic Compute Cloud(EC2) Instance

## What is an Amazon EC2 instance?
An Amazon EC2 instance is a virtual server in Amazon's Elastic Compute Cloud (EC2) for running applications on the Amazon Web Services (AWS) infrastructure. Amazon provides various types of instances with different configurations of CPU, memory, storage and networking resources to suit user needs. Each type is available in various sizes to address specific workload requirements.

- Sign into the AWS management console and ensure that you are in the N.Virginia(us-east-1) region.
- To create your EC2 instance, go to Amazon EC2 in the ***[AWS Management Console](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#Home:)***. Choose the ***Launch instance*** button to open the instance creation wizard.

![Screenshot 2023-01-20 at 09 53 00](https://user-images.githubusercontent.com/50238769/214065241-867579e4-10dc-4287-a41b-d746debad4cc.png)

- On the first page, enter **wordpress-server** as your instance name.
- Select the ***Amazon Linux 2 AMI (HVM)*** in the AMI selection view. Amazon EC2 allows you to run 750 hours per month of a t2.micro instance under the  ***[AWS Free Tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)***. Select this option for this guide so that you won’t incur any costs on your bill.

![Screenshot 2023-01-20 at 09 57 30](https://user-images.githubusercontent.com/50238769/214065933-c70e2e19-1fa0-4447-ae30-6e7ff035f877.png)

## Create Key Pair

### What is a key pair?
A key pair, consisting of a public key and a private key, is a set of security credentials that you use to prove your identity when connecting to an Amazon EC2 instance. 
- Give your key pair a name. Then choose the ***Create key pair*** button, which will download the ***.pem file*** to your machine. You will use this file in the next module.

![Screenshot 2023-01-20 at 10 06 35](https://user-images.githubusercontent.com/50238769/214070582-828a4b74-a05e-45dc-897f-b5cf511c6be0.png)

## Configuring a security group and launching your instance. 

### What is a security Group?
A security group acts as a virtual firewall for your EC2 instances to control incoming and outgoing traffic. Inbound rules control the incoming traffic to your instance, and outbound rules control the outgoing traffic from your instance. 

You need to configure a security group before launching your instance. Security groups are networking rules that describe the kind of network traffic that is allowed to your EC2 instance. You want to allow two kinds of traffic to your instance:
- SSH traffic from your current IP address so you can use the SSH protocol to log in to your EC2 instance and configure WordPress. 
- HTTP traffic from all IP addresses so that users can view your WordPress site.

To configure this, select ***Allow SSH traffic from My IP*** and select ***Allow HTTP traffic from the internet***.

![Screenshot 2023-01-20 at 10 25 42](https://user-images.githubusercontent.com/50238769/214075441-f8139b56-7dda-4c14-bcb2-bf176d31edac.png)

![Screenshot 2023-01-20 at 12 10 01](https://user-images.githubusercontent.com/50238769/214075525-bf5d66d0-fa25-4912-9103-58be064bb630.png)

## Launch EC2
- Choose the ***Launch instance*** button to create your EC2 instance.


# Module 3: Configuring Your Amazon RDS Database

## Database security methods
- It is critical to secure your database from unauthorised access, and there are a number of strategies you can use to add security to your database. You will learn two of them in this module. They are:
- ***Network security***: Limiting access to your database instance by rejecting traffic that’s not from authorised IP addresses
- ***Password authentication and authorisation***: Limiting access to your database by requiring a username and password to access.

## Allow your EC2 instance to access your Amazon RDS database

First, you will modify your Amazon RDS database to allow network access from your EC2 instance.
In the previous module, you created security group rules to allow SSH and HTTP traffic to your WordPress EC2 instance. The same principle applies here. This time, you want to allow certain traffic from your EC2 instance into your Amazon RDS database.
- To configure this, go to the Amazon RDS databases page in the AWS console. Choose the MySQL database you created in the earlier module in this guide.

![Screenshot 2023-01-20 at 13 51 55](https://user-images.githubusercontent.com/50238769/214079034-3ff22088-0998-4ada-bdda-f0865ef175d9.png)


- Scroll to the ***Connectivity & security*** tab in the display and choose the security group listed in ***VPC security groups***. The console will take you to the security group configured for your database.


![Screenshot 2023-01-20 at 13 55 58](https://user-images.githubusercontent.com/50238769/214079388-82cddd9f-5ea5-470d-9996-ed603323466a.png)

- Select the '***Inbound rules'*** tab, then choose the ***'Edit inbound rules'*** button to change the rules for your security group.


![Screenshot 2023-01-20 at 14 10 15](https://user-images.githubusercontent.com/50238769/214080818-4869bb3e-6558-4a26-91c6-ed812bd069a5.png)

- The default security group has a rule that allows all inbound traffic from other instances in the default security group. However, since your WordPress EC2 instance is not in that security group, it will not have access to the Amazon RDS database. Change the ***Type*** property to ***MYSQL/Aurora***, which will update the ***Protocol*** and ***Port range*** to the proper values. Then, remove the current security group value configured for the ***Source***.
- For ***Source***, enter wordpress. The console will show the available security groups that are configured. Choose the ***wordpress*** security group that you used for your EC2 instance.
- After you choose the ***wordpress*** security group, the security group ID will be filled in. This rule will allow MySQL access to any EC2 instance with that security group configured.
- When you’re finished, choose the Save rules button to save your changes.

![Screenshot 2023-01-20 at 14 26 50](https://user-images.githubusercontent.com/50238769/214081558-70545cdb-7a34-4d1a-9500-23a3524b09d6.png)

## SSH into your Instance

### What is SSH and why it used?

SSH (Secure Shell) is a network protocol used to securely connect to a remote computer or server. It encrypts all data sent over the network, including passwords, preventing them from being intercepted by malicious actors. SSH allows for remote access to the command line interface of a remote system, making it possible to perform tasks and manage files on a remote machine. It also supports tunneling, forwarding TCP ports and X11 connections. SSH is a widely used and powerful tool that provides secure and efficient remote access to computer systems and servers, allowing for easy and secure management of remote resources.

- Now that your EC2 instance has access to your Amazon RDS database, you will use SSH to connect to your EC2 instance and run some configuration commands.
- Go to the ***[EC2 instances page](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:)*** in the console. You should see the EC2 instance you created for the WordPress installation. Select it and click on the connect button. 
- Select the ***‘SSH client’*** tab and follow the instructions.


![Screenshot 2023-01-20 at 15 44 22](https://user-images.githubusercontent.com/50238769/214083293-e3eba386-ac6a-4181-99a1-d62d7cb6d106.png)

- Previously, you downloaded the ***.pem file*** for the key pair of your instance. Locate that file now. It will likely be in a Downloads folder on your desktop.Open a terminal window. If you are on a Mac, you can use the default Terminal program that is installed, or you can use your own terminal. 
- Copy and execute the commands below
- In your terminal, run the following commands to use SSH to connect to your instance. Replace the ***“<path/to/pem/file>”*** with the path to your file, e.g., ***“~/Downloads/wordpress.pem”***, and the ***“<publicIpAddress>”*** with the public IP address for your EC2 instance. 


``` 
  chmod 400 wordpress.pem
  ssh -i "wordpress.pem" ec2-user@ec2-204-236-240-173.compute-1.amazonaws.com
```
<img width="1251" alt="Screenshot 2023-01-20 at 16 35 32" src="https://user-images.githubusercontent.com/50238769/214085401-7bfe7a0c-d785-4527-9294-0286247855e4.png">
  
## Create a database user

- You should have an active SSH session to your EC2 instance in the terminal. Now, you will connect to your MySQL database. First, run the following command in your terminal to install a MySQL client to interact with the database. 

<img width="1275" alt="Screenshot 2023-01-20 at 17 36 32" src="https://user-images.githubusercontent.com/50238769/214085824-f4563976-5b63-46da-9cfa-343d2dde306e.png">

- Go to the ***[Amazon RDS databases page](https://us-east-1.console.aws.amazon.com/rds/home?region=us-east-1#databases:)*** in the AWS console. You should see the wordpress database you created for the WordPress installation. Select it to find the hostname for your Amazon RDS database.

![Screenshot 2023-01-20 at 17 38 38](https://user-images.githubusercontent.com/50238769/214086148-618bad0b-a2ee-4710-a047-80d51e327259.png)

- In the details of your Amazon RDS database, the hostname will be shown as the ***Endpoint*** in the ***Connectivity & security*** section.
  
![Screenshot 2023-01-20 at 17 39 13](https://user-images.githubusercontent.com/50238769/214086282-c5402df8-7ce6-4d2a-a7b0-17216b555fa4.png)

- In your terminal, enter the following command to set an environment variable for your MySQL host. Be sure to replace “<your-endpoint>” with the hostname of your RDS instance. 

```
  export MYSQL_HOST=<your-endpoit>
```  

<img width="1271" alt="Screenshot 2023-01-20 at 17 41 48" src="https://user-images.githubusercontent.com/50238769/214087363-3d7ebe28-1c0f-4471-b46d-ff61a2a17d9a.png">
  
- Finally, create a database user for your WordPress application and give the user permission to access the wordpress database. Run the following commands in your terminal:  

```
CREATE USER 'wordpress' IDENTIFIED BY 'wordpress-pass';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpress;
FLUSH PRIVILEGES;
Exit
```
As a best practice, you should use a better password than wordpress-pass to secure your database.
Write down both the username and password that you configure, as they will be needed in the next module when setting up your WordPress installation.  

# Deploy WordPress with Amazon RDS
  
  
## Installing the Apache Web Server

- To install Apache on your EC2 instance, run the following command in your terminal:

```
sudo yum install -y httpd
```
- To start the Apache web server, run the following command in your terminal:
  
```
sudo service httpd start
```

- You can see that your Apache web server is working and that your security groups are configured correctly by visiting the public DNS of your EC2 instance in your browser.
  
- Go to the [EC2 Instances page](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:) and find your instance. Go to your instance destails tab and copy the ***Public IPv4 DNS***

# Download and configure WordPress

- First, download and uncompress the software by running the following commands in your terminal:

```
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
```
  
- If you run ls to view the contents of your directory, you will see a tar file and a directory called wordpress with the uncompressed contents.

```
ls
latest.tar.gz  wordpress
```
- Change the directory to the wordpress directory and create a copy of the default config file using the following commands:

```
cd wordpress
cp wp-config-sample.php wp-config.php
```
- Then, open the ***wp-config.php*** file using the nano editor by running the following command.
  
```
nano wp-config.php
```  
- You need to edit two areas of configuration. First, edit the database configuration by changing the following lines:  
  
```  
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );  
```  
  
  
The values should be:
- ***DB_NAME***: “wordpress”
- ***DB_USER***: The name of the user you created in the database in the previous module
- ***DB_PASSWORD***: The password for the user you created in the previous module
- ***DB_HOST***: The hostname of the database that you found in the previous module  
  
  
The second configuration section you need to configure is the Authentication Unique Keys and Salts. It looks as follows in the configuration file:  
  
```  
/**#@+
 * Authentication Unique Keys and Salts.
 *
 * Change these to different unique phrases!
 * You can generate these using the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}
 * You can change these at any point in time to invalidate all existing cookies. This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );  
```  
Go to this ***[link](https://api.wordpress.org/secret-key/1.1/salt/)*** to generate values for this configuration section. You can replace the entire content in that section with the content from the link.

You can save and exit from nano by entering ***CTRL+O*** followed by ***CTRL+X***.

With the configuration updated, you are almost ready to deploy your WordPress site. In the next step, you will make your WordPress site live.  
  
  
  
  
  
  
  
  
  
  

# Create Elastic Load Balancer Security group

In the Network settings section create security group inbound rules. Security groups are stateful, meaning any changes applied to an incoming rule will be automatically applied to the outgoing rule.
  - HTTPS - Allow HTTPS traffic from anywhere(0.0.0.0/0).
  - HTTP - Allow HTTP traffic from anywhere (0.0.0.0/0).
- Configure Storage to be 20 GiB.
- Continue through the setup keeping the defaults and launch the instance.
- Add these rules in the screenshot to your security group. 
![Screenshot 2022-11-08 at 13 26 46](https://user-images.githubusercontent.com/50238769/200865784-0ad0c5cf-233a-4216-af78-bb6e02ca56ac.png)

# Add Application Load Balancer
- Elastic Load Balancing automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones. It monitors the health of its registered targets, and routes traffic only to the healthy targets. 
- Steps to creating an Application Load Balancer follow this link - https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html.
- Go to the Elastic Compute Cloud dashboard and look for the 'Load Balancing' tab, Select 'Load Balancer' and create your application load balancer.
![Screenshot 2022-11-09 at 18 26 30](https://user-images.githubusercontent.com/50238769/200886068-e654580b-d5a9-4102-ac08-c315393aaabc.png)


- Each target group routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number that you specify. You can register a target with multiple target groups. You can configure health checks on a per target group basis. Health checks are performed on all targets registered to a target group that is specified in a listener rule for your load balancer.
- Register your EC2 in the Target Group target group you created. 

![Screenshot 2022-11-09 at 18 14 37](https://user-images.githubusercontent.com/50238769/200882546-92f8c531-4a62-4964-bbac-c3d6310d99ff.png)

- Use the elastic load balancer security group for your Application Load balancer. 

# Make your EC2 security groups more secure.
- The security groups should not open traffic to 0.0.0.0 (internet). 
- Only the ELB should allow traffic to 0.0.0.0, the EC2 security group should only allow traffic to and from the Load Balancer security group. 
- Edit inbound rule and attach HTTPS and HTTP to ELB security group. 

![Screenshot 2022-11-09 at 18 38 15](https://user-images.githubusercontent.com/50238769/200889128-0ac4646b-05a9-4cbc-a959-38e033dfd97a.png)


# View Website
- Copy your Application Load Balancer DNS Name and paste the link on your browser to view your website.

<img width="1440" alt="Screenshot 2022-11-25 at 16 22 33" src="https://user-images.githubusercontent.com/50238769/204005321-f6bdeeb0-9338-4e91-8a1f-727553d8c081.png">


Thanks for reading 👍! Please leave feedback and let me know if you enjoyed this post. 

