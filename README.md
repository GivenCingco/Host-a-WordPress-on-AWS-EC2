# WordPress website hosted on an Amazon EC2 instance

You must have an existing AWS account. Every resource we use will be under [Free tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all) 

- Sign into the AWS management console and ensure that you are in the N.Virginia(us-east-1) region.
- Go to the EC2 [dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#LaunchInstances:) and click the orange 'Launch Instance' button.

![Screenshot 2022-11-22 at 15 22 43](https://user-images.githubusercontent.com/50238769/203331652-41a764ab-3ebf-45a6-a9e4-2dbd1997f9aa.png)

- Select the Ubuntu SERVER 22.04 lts (hvm), ssd Volume Type AMI

![Screenshot 2022-11-22 at 15 37 44](https://user-images.githubusercontent.com/50238769/203333206-f9a5e284-dcd5-4d32-bb73-904f3b612aa4.png)
  - Give the server a name 
  - Select the t2.micro istance type (free tier eligible).
  - Create a new key pair
      -Name your keypair.
      - Type and format must be RSA and .pem file format.

- In the Network settings section create security group
  - SSH - Allow SSH traffic from anywhere(0.0.0.0/0)
  - HTTPS - Allow HTTPS traffic from anywhere(0.0.0.0/0)
  - HTTP - Allow HTTP traffic from anywhere (0.0.0.0/0)

- Configure Storage to be 20 GiB
- Continue through the setup keeping the defaults launch the instance.
