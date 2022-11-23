# WordPress website hosted on an Amazon EC2 instance

You must have an existing AWS account. Every resource we use will be under free tier. Free tier: In your first year includes 750 hours of t2.micro (or t3.micro in the Regions in which t2.micro is unavailable) instance usage on free tier AMIs per month, 30 GiB of EBS storage, 2 million IOs, 1 GB of snapshots, and 100 GB of bandwidth to the internet.
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
  - SSH - Allow SSH traffic from your machine IP Address by selecting 'My IP' under source.
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
- Follow these commands below:
  - Type '***chmod 400[Key pair name]***' and press enter. 
  - Type '***ssh ubuntu@[Public IPv4 address] -i [Key pair name]***' and press enter. Type yes when prompted if you want to continue connecting. 
  - Type '***sudo su -***' to have root priviledges. 

## Update and upgrade LAMP packages. 
Type the following commands in your terminal:
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

  


