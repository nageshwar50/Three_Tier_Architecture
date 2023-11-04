# Architecture of the Project
![AWS Cloud](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/60d8ffd5-380b-47a5-83e6-2f1fc9aa1a19)
# A Step-By-Step Guide
## VPC
firstly we are going to set up VPC in both US East (N. Virginia). The below image contained all the subnets, their IP range, and their uses.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/5d39c375-c659-42e6-95c9-12621cd793ec)

Please log in to your AWS Account and type VPC in the AWS console. and click on VPC service. Click on Your VPC's button on the left and then click on Create VPC the button on the top right corner of the page and give please enter the name that you want to keep and the IPV4 CIDR block. in my case CIDE block is 172.20.0.0/16
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/574e56f2-45ed-4cce-909f-94c26131c9a6)

## Subnet
now click on the subnet button which is located on the left side and then click on the Create subnet button on the top right corner of the page.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/988b6abd-a9b3-4c64-9aca-493acefda2c3)

please remove the default VPC ID and choose the VPC ID that we have just created in the VPC ID field. and click on the Add Subnet button at the bottom. now we need to configure our subnets. Again you can use the VPC configuration image that I shared earlier on the blog to get the IP range and to know which subnet will be used for what purpose. we are going to create a total of 8 subnets of which 2 of them are public and the rest of 6 subnets are private. you can create a subnet as I have shown in the below image. after adding all the subnets click on Create Subnet button.after the successful creation of all 8 subnets, they look like this. you can verify with my subnets.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/90585a08-87b0-4e1b-9c26-56c5310dbddc)

## Internet Gateway
Now we are going to create Internet Gateway also known as IGW.  it is responsible for communication between VPC, VPC's public subnet with the Internet. without IGW  we won't be able to communicate with the Internet. so let's create that. click on the  internet gateways button on the left panel. and then click on the Create Internet gateways button on the top right corner of the page.give any name you wanna give to IGW. and click on the Create Internet gateway button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/660d0768-be32-4f47-9d5e-e6711b912d3d)
after creating an internet gateway we need to attach it with VPC to use it.  for that click on the 	Action button. here you can see the drop-down list. please select the option Attach to VPC.please select VPC that we have created just now from the Available VPC list. and then click on the Attach Internet gateway button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/20b91243-01ec-4b8a-964f-dba409ae215b)

## NAT gateway 
Now we need to create a NAT gateway. NAT gateway is responsible for connecting resources that are in the private subnet to communicate with the internet. all the resources that will be there in a private subnet will communicate to the internet through the NAT gateway. we will keep the NAT gateway in the public subnet so that it can access the internet. NAT gateway is a chargeable resource. so you will be charged by AWS as long as you keep it up. Now to create a NAT gateway click on the NAT gateways button on the left panel of the web page. and then click on the Create NAT gateways button in the top right corner of the page.
give any name you want to give to the NAT gateway. but be cautious with selecting a subnet. You have to select one of the Public subnets among the two. either pubsub-1a or pubsub-2b. then click on the Allocate Elastic IP button to allocate Elastic IP. and then click on the Create NAT gateways button. NAT gateway creation takes 2-4 minutes.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/cb1da872-f0e5-4e4c-92bc-c59032bda3b6)

Now we need to have a route table to handle traffic for public subnet and private subnet and for that, we need to create a Route table. we are going to create two route tables one for the public subnet and another one for the private subnet. first, we are going to create RT for the public subnet. so click on the Route table button which you can see on the left panel. and click on the Create Route table button on the top corner of the page.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/b94645e3-6432-4c76-8731-05ad4bbec7f7)
let's create RT for the private subnet.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/6ca75c83-01a6-463a-99ce-99c0c0e016f1)
Now, we need to do some association with both RTs so select Pub-RT and click on the Routes tab at the bottom and then click on the edit route button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/7119d10e-b175-4e1b-bddc-60d4df262d92)
click on the Add Route button. and select 0.0.0.0/0 in the destination field. and then click on the Target field. As soon as you click on the Target field one drop-down will open and here you have to select Internet gateway, shown in the below image.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/73a0e244-c488-4887-a04f-fe0ef3d80013)
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/031f944e-4bc5-4487-a469-80412d71d2a0)

keep Pub-RT selected and click on the Subnet associations tab next to the Routes tab. and then click on the Edit subnet associations. as shown in the below image.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/239dd06b-7e0f-405a-9b08-a590b6000c88)
now select both public subnets. pubsub-1a and Mypubsub-2b.  and click on the save associations button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/2235b8e4-b943-464d-bdf3-9ce8159a7c83)

now we have to do the same thing for the prvt-RT as well. but there is one slight change. let me show you. Please select prvt-RT and click on the Routes tab at the bottom of the page.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/4ea555f5-8f92-4919-8083-0fa2520c6bec)
Here please select 0.0.0.0/0 in the destination field and click on the target. As soon as you click on the target you will see the drop-down list. Please select NAT gateway from the drop-down list. As shown in the below image. select the NAT gateway that we have just created. and click on the save changes button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/a535b4a9-8fa0-4311-9c9d-d047cb102451)
keep Prvt-RT selected and click on the subnet associations tab at the bottom next to the Routes tab. And then click on the Edit route associations button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/949697ee-2844-4814-a548-fe6b05023898)
Here you can see the same situation as we saw before. But here we are going to select all the 6 private subnets. And then click on the save association button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/5677ab6c-cf45-4598-99e7-af51d8abca9e)

Before we move ahead I want to change the settings of VPC and two public subnets. So just click on the Your VPC button on the left panel and select VPC that we have created and click on the action button and there you will see the drop-down menu. Select the Edit VPC setting button.And here please enable Enable DNS hostname checkbox by clicking on it. and then click on the Save button
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/b3fcb76e-df8b-4979-bba3-ccba068c75bc)

Please go to the subnet page and select the public subnet and click on the action button and then choose the Edit subnet setting button from the drop-down list.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/828face9-2ccf-434a-afec-2ed022393fa0)
Here you have to mark right on Enable public assign public IPV4 address. And then click on the save button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/473b5afe-e3a5-4428-a11f-774b2ead769c)

And here we are done with VPC configuration in the primary region. In my case us-east-1 (N.Virginia). I hope you did the setup. Now let’s move ahead...

## Security Groups (SG)

Security groups are very essential part of the infrastructure. Because it can secure the resources in the cloud. SGs are a kind of firewall that allow or block incoming and outgoing traffic. SGs are applied to the resources like ALB, ec2, rds, etc. One resource can have more than one SG.
So let's first understand. How SG will be used in our architecture and how we are going to apply that. Please see the below image you will get all the ideas. Which resource depends on what. And what are the port numbers we need to allow etc.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/09dfea5b-d623-4509-abd0-f2ad11e4b9a2)

To create SG, click on the security groups tab on the left panel and here you will see the Security Groups button. Note that SGs are specific with VPC. So we can’t use SG which is created in a different VPC. So when you create SG please make sure that you choose the right VPC. click on the crate security button on the top right corner. We will create our first SG for bastion-jump-server. Give any name and description you want but please remove the default VPC  and add VPC that we have just created. Then click on the Add rule button in inbound rules. And add SSH rule and add your IP in the destination. Please don’t do anything with the outbound rule if you don't have a good understanding. And then click on the create security group button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/63bd2da1-83f6-4a74-a599-d857be046cad)

Now let's create SG for the ALB-frontend. Again steps are similar but add the rule HTTP AND HTTPS from anywhere on the internet because both ALB are internet facing. But please select the right VPC
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/88c83be0-ed10-40ad-a8f3-f9c66c3e5119)

Create SG for ALB-backend. ALB-backend is also internet-facing. Again allow HTTP and HTTPS from anywhere.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/3fcccd86-8091-48be-be90-1c922ca70a67)
create SG for frontend servers. Our fronend server will be in a private subnet so add the HTTP rule and select the source as ALB-frontend-sg.  So only ALB-frontend can access the frontend server on port 80. You have to add one more rule SSH allows from bastion-jump-server-sg. So that the bastion host can log in to web servers.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/80c38ebf-0281-40f7-9fd0-2df03caf5214)

Let's create SG for the backend server. Again steps are completely similar to frontend-sg. You have to allow port 80 from ALB-backend-sg so that only ALB-backend can request to the backend server and add the rule SSH allows from bastion-jump-server-sg. So that the bastion host can log in to backend servers.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/9b50fc82-d056-4548-9ba1-01c7cba8bb91)

Lastly, we are going to create SG for RDS instance. Allow port 3306 MySql/Arrora from backend-sg so that only the backend server will be able to access it. and no one else can access our database.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/afdbf380-fad7-4304-9a26-307c7ccbb8f9)
And here our SG setups are complete now.

##  RDS
Now we are going to set up a database for our application. And for that, we are going to utilize the RDS service of AWS. So let's head over to the RDS dashboard. Just search RDS in the AWS console. 
Now first we need to set up a subnet group. It specifies in which subnet and Availability zone out database instance will be created. So click on the subnet group button on the left panel. And click on the button Create database subnet group which is in the middle of the web page.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/5c302ed1-3396-4cf5-9cd0-940514d945ac)
Here we can configure our VPC, subnet, and availability zone.  Give any name to your subnet but make sure you select the correct VPC. and select Azs us-east-1a and us-east-2b. According to the architecture that I have shown you, our database will be in private subnet  prvtsub-7a and prvtsub-8b. so please select as I have shown in the below figure. And then click on the create button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/a4c1ac10-5da1-4f26-8644-21a2ca4bc247)
we are going to create a database. So click on the database button on the left panel and then click on the created database button.
On this page, we can configure our database. Select standard create because I’m going to show you each and every step.  select MySQL in the engine option because our application runs on MySQL database. If your app runs on other engines you can select that one. Furthermore, you can select the engine version my application is compatible with MySQL version. But you can select according to the developer guild.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/69d37777-dc93-41cd-b9d2-8e6a1b299b1e)

Scroll down, and select Dev/test as  template. If you select the free tier then you won’t be able to deploy RDS in a multi-availability zone.  Select Multi-AZ DB instance from availability and durability option. In settings give any name to your database.  In the credential setting give the username of the database in the Master username field and give the password in the Master password field. And then confirm the password below. Please do remember your username and password.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/9ce96469-8a64-4ad8-9c68-e472c79802d0)

Again scroll down, select Brustable class in the instance setting and select the instance type. Actually, it depends on your application uses. But for learning purposes, I am selecting t3.micro.  now in storage type select General purpose(GP2) and allocate 20 GiB for database. Please uncheck the auto-scaling option to keep our costs low.  And In the connectivity option please select the option according below screenshot.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/02d1975e-e299-46cb-a658-7cda8c42da42)
In VPC, select VPC that we created earlier and in DB subnet group select the group that we just created, In the public access option please select No, choose existing security, and select security group rds-sg.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/df02c925-1e55-4e92-b032-709834545c13)
Scroll down, click on Additional Configuration, and in the database option give the name test because we need a database with the name of the test in the application.  Enable Automated Backup. Note: you have to enable automated backup otherwise you won’t be able to create a read replica of the RDS instance
Scroll down, mark on enable encryption checkbox to make the database bit more secure, and click on Create database button below.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/3f552b61-dc2f-437a-b85a-221ef13fc6fc)

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/9200ac99-06e3-4ad7-af75-ba296632b1dd)

## Note:
RDS take 15-20 minute because it creates a database and then take a snapshot. So please have patience and wait for it to be ready
After your database is completely ready and you see the status Available then select the database and click on the Action button. There you can see the drop-down list. Please click on created read-replica.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/08afcf70-ae5f-4565-8d49-fc0628f96c47)

This page is similar to creating a database. In the AWS region select the region where you want to create the read replica. In my case, It is Oregon (us-west-2).  Give a name to your read replica, and select all the necessary configurations that we did before while creating the database. For your reference, I have shown everything in the below images.

This page is similar to creating a database. In the AWS region select the region where you want to create the read replica. In my case, It is US East (N. Virginia) us-east-1.  Give a name to your read replica, and select all the necessary configurations that we did before while creating the database. For your reference, I have shown everything in the below images. You can check your read replica on the specified region’s RDS dashboard.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/2bd1fa75-2a1a-4c6d-bd39-4a1d7df42fcd)

## Note:
we can’t write anything into a read replica. It is just read-only database. So when a disaster happens we just have to promote read replica so that it becomes the primary database in that region.


## Route 53
Now we are going to utilize route 53 service and create two private hosted zone.  for north Virginia(us-east-1 Firstly, we are gonna create a hosted zone for us-east-1. Click on the Hosted Zones button on the left panel and click on the created hosted zone button on the top right corner.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/f569d641-1405-46b9-a012-db63ec036d00)

Give any domain name because anyhow it will be private hosted zone but it would be great if you give the name same as mine (rds.com). Please select the private hosted zone and Select the region. In my case, it is us-east-1. And then select VPC ID. Make sure you select VPC that we created earlier. Because this hosted zone will resolve the record only in specified VPC.  and then click on the Create hosted zone.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/7a7c7e64-9319-44c3-8403-6767ec046712)

Click on the defined record button in the middle of the box.Here type book in the record name field. In the record type select CNAME. In the value field paste endpoint of the RDS which is in us-east-1. Then click on the defined record button.After successfully completing the above steps your Route 53 console look like this.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/403945c5-acaf-42ed-acb8-fc0195536c29)


## Certificate Manager

I have the domain name nageshwarpandey.co.in in Route 53. Now I am going to use this domain name to create subdomains such as api.nageshwarpandey.co.in  and that will resolve ALB-backend DNS. Furthermore, we need an SSL certificate so that we can make the connection secure.

## Note: 
I have wildcard  certificates
## Application Load balancer(ALB) and Route 53
Now it’s time to set up an Application load balancer. We need two load balancers, one point to the backend server, and another point to the frontend server.
Note: before we created ALB we need to create a Target group(TG). So first we will create TG for ALB-frontend and then create TG for ALB-backend.

Type ec2 in the AWS console. and click on the EC2 service.Click the target group button on the bottom of the left panel. And click on the create target group button in the middle of the page.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/b140963c-3ad0-4771-bbdb-2bbb57fa90d5)

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/432501f1-c36d-4db3-a0b3-20f619680915)

Click on the create target group button.

Let's create TG for ALB-backend. Click on the create target group button. Select the target type Instance. Again give some meaning full name such as ALB-backend-TG. Select VPC that we have created and after that we can see that we have two TG. ALB-frontend-TG and ALB-backend-TG.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/587e3a4a-97a9-4087-ba16-700caf6e1aec)

Now let's associate these TG with the load balancer. So click on the Load Balancer button at the bottom of the left panel and click on the create load balancer button. First, we will create ALB for frontend.
Choose Application load balancer and click on create button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/ea115a7d-31a4-4f62-bd88-1dd41625d3e0)
here we can configure our ALB. First, give the relevant name to ALB such as ALB-frontend. Select the internet-facing option. In Network mapping select VPC that we have created. Select both availability zone us-east-1a and us-east-2b. and select subnet pubsub-1a and mypubsub-2b respectively Scroll down and click on the create load balancer button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/c64036e1-1999-42bb-9570-084aabf761f7)

Now, lets create ALB for backend. Again choose Application  load balncer option and click on the create button.
Select Internet facing option. And select VPC that we have created. Select both availability zone us-east-1a and us-east-2b.  and select subnet pub-sub-1a and pub-sub2b. select security group ALB-backend-sg that we created for ALB-backend. And in the listner part select TG that we just created ALB-backend-TG.Scroll down as click on the Created Load balancer button 


Now we have two load balancers, ALB-frontend and ALB-backend. But we need to add one more listener in ALB-backend. So click on ALB-backend.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/4cd5862e-0a4e-4e97-9fde-1d8053064174)

selcet ALB-Backend and Click on add listener the button that is located on the right side.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/d9d7e3c8-9c37-42df-beb7-b102dcfdfbfc)

Here In listener details select HTTPS. Default Action should be Forward and select ALB-backend-TG. Now we need to select the certificate that we have created. So in the Secure Listener setting select the certificate.  And click on the add button below.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/55177ec3-2202-461f-95ed-c1a73183c2c5)

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/6745c531-5520-42f5-9d9f-6a8c98b9edde)

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/7ad6a961-c123-403c-9214-89888b82f1db)

here is my wildcard ssl which i have imported on aws.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/99a3580a-091e-4213-ab9c-b3bef2e72732)

## I hope that you have also completed these step

## EC2
Now we are going to create a temporary frontend and backend server to do all the required setup, take snapshots and create Machine images from it. So that we can utilize it in the launch template. It is a long process so bear with me.
First, click on the instance button and then click on the Launch Instance button on the top right corner.
First, we are going to set up a frontend server. Give a name to your instance (temp-frontend-server). Select Ubuntu as the operating system. Choose the instance type as t2.micro.  click on Create key pair if you don’t have it.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/4fbed5b0-a3a5-47be-8315-4e9ad067cc4b)

If you are creating key pair make sure you select .PEM file format as I have shown in the below image. Because we are going to use  to do the ssh  and give any name to your key. And save it somewhere safe location on your computer

Here we are doing a temporary setup so we don’t use our OWN VPC. we can use the default VPC given by AWS. In short, keep the Network setting as it is. In the firewall setting select all the fields as I shown in the below image to keep things simple. And lastly, click on the Advance details option.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/366d0f99-bebf-4d7d-837b-f66488de4a55)

Scroll down to the bottom of the page, here we can see one text box with the name USER DATA. Here in this text box, you can write your bash script file and that will be executed during the launch of the instance. I have given the bash script below. so please copy that script and paste it here. And lastly, click on the launch instance button.
you can use userdatata.sh code.

we have successfully launched temp-frontend-server. so now let’s launch a temporary backend server.  give a name to your instance (temp-backend-server).  select ubuntu as the operating system. And select t2.mirco as instance type. Here we don’t have to create a new key, we can utilize the previous key that we have created while launching the frontend instance.
Scroll down to the bottom of the page, and copy the bash script that I have given backuserd.sh . and paste it in the USER-DATA text box. This bash scripting installs some packages so that we don’t have to install them manually. And click on the launch instance.

Please wait for 5-8 minutes so that the instance comes in a running state. and then we will utilize instances for further steps.
Select temp-frontend-server. and copy the IP address of the instance. Now open the Terminal where you have downloaded your YOUR_KEY.pem file. And type the command.
ssh -i <name_of_key>.pem ubuntu@<Public_IP_add_of_Instance> like.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/1eb8c88d-f665-44f6-a13f-0268d32146a3)

Now you are successfully logged your remote temp-frontend-server. now our first task is to clone my git repo. If you are working on your own project then clone your repo. So type the command in the terminal.

after done ssh clone this repo

git clone https://github.com/nageshwar50/Three-tier_code.git
cd Three-tier_code/client 

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/dadaf980-ba02-40c0-8378-640db2fae032)

Now, we need to change just one line in our frontend application that is built in React. So type the command
vi src/pages/config.js
The above command opens the file in a text editor. Now press I the button on your keyboard to edit the file. In this file, we have to change API_BASE_URL. So remove whatever is present in the API_BASE_URL variable.remove this IP
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/87885e8e-068f-49e2-8781-aa27c9a62798)

And add https://api.nageshwarpandey.co.in, In my case I have added this URL but in your case it is different. This means you need to use your OWN domain name. so your API_BASE_URL should be like https://api.<YOUR_DOMAIN_NAME>.XYZ I hope it makes sense.  After updating the variable press ESC key on your keyboard and then type :wq and hit the Enter button.
After making these changes our frontend of the application will send all the API calls on the domain name https://api.nageshwarpandey.co.in And lastly, that will point to our backend server.
Now type the command npm install in the terminal to install all the required packages. < npm install  >
Type the command npm run build to create the optimize static pages. < npm run build > 

Now you have one more folder in the directory called build. You can verify that by tying ls command
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/cd242e50-d128-43aa-981d-74729665d42e)

Now type the very essential command sudo cp -r build/* /var/ww/html/

sudo cp -r build/* /var/www/html
The above command takes all the static files from the build folder and stores them in /var/www/html so that Apache can serve them.
Here our temp-frontend-server configuration is completed. Now let's set up the temp-backend-server. So select the temp-backend-server and copy the IP address of the instance.  Again please open Git bash in the same directory where your stored key.pem file. And type the below command.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/81a0df07-b919-40e6-a7ff-8591e15b7de0)
We are successfully logged in inside the backend server. first, we will clone the repo
git clone  https://github.com/nageshwar50/Three-tier_code.git
go inside the Three-tier_code/backend
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/d32f1db6-ca00-417e-8368-9d6a3fa1f736)

vi .env
Press the I button on your keyboard. And copy the code given below and paste the snippet into the code editor. This code contains information about the RDS instance. Please change your username and password according to whatever you kept while creating a database. And then click on the  ESC button and type :wq and hit the enter button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/c1fa4420-57d2-4a22-85cd-7622ee0257b0)
Now type the below commands in terminal

npm install 
npm install dotenv

Now, let's start the backend server.  ( very IMP )

sudo pm2 start index.js --name "backendApi"

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/690652f8-cd36-48e3-a3c3-22926df3242b)
Yeah!!!😃 Successfully completed our backend server configuration. You can close the terminal

But before we end this section we need to do a few more steps. We have to create Machine images of these servers so that we can create a launch template. these steps are optional because anyhow we will take a backup from the AWS backup service and that will do the same thing. but that takes time. so it would be better if you follow the steps.

So please select temp-frontend-server and click on the Action button in the top right corner. One drop-down menu will open. You have to select the images and template option and that will give one more drop-down menu from which we need to click on create image button.Give the name you your image (img-frontend-server). just deselect that delete on the termination button and click on the create image button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/643b9d7b-63df-43f3-99e1-6ca6cea68f4f)

You have to do the same thing for the temp-backend-server as well. I have shown you each and every step in the below images.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/8ee2336f-1742-427a-98ec-966cf2c9927f)

After a couple of minutes (10-15) you can see those images. Click on the AMIs button on the left panel and you can see both images here. its right now in pending state 
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/23251ab6-bf79-492c-8722-bae340c28387)

##  Launch Template 
Take note that we need to create a launch template. So click on the launch template button on the left panel and click on the create launch template button.
Give the name to your launch template such as template-frontend-server as we are creating a launch template for frontend-server. let's give the version 1 in the version field. Here we need to select AMI so click on My AMIs tab and select the option owned by me. So now it will show you all the images that are present in your current region.  If you are following the blog from starting then you will have a total of 4 images in N.virginia. coz two we created manually and two were created by backup service.  Here you have to select the image that contains the frontend application. Either you can select the manual or the one created by the backup service. both are okay coz it contains the same data. Select instance type t2.micro

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/3b4ae5f0-986f-4324-aaaa-7578f752adec)
Scroll down, attach the key pair, and in the network setting just select the security group that we created for the frontend server. in my case the name SG is frontend-sg.  And click on the advance details section at the bottom of the page.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/e5bac00b-0176-4be0-aa39-6589ff3a1763)
Scroll down to the bottom, and in the USER-DATA text box paste the code that I have given below. And then click on the Create launch template button.
#!/bin/bash

sudo apt update -y

sleep 90

sudo systemctl start apache2.service 
we successfully created a launch template for the frontend-server. now let's create a launch template for the backend server.

Give a name to your launch template (template-backend-server). Give version 1 in the version field, but make you select the correct AMIt that holding your backend application.  And Select an instance type t2.micro


![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/ac1de6be-1cc5-4074-b303-a15c6a70a816)
 userdata
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/e65f4f47-41dd-4545-98bd-e188821b957a)

We have created two launch templates, template-frontend-server and template-backend-server
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/dd34d9b8-bd79-4694-9cc0-18da39f4a75d)

## Auto scaling group (ASG)
The auto-scaling group is the functionality of EC2 service that launches instances depending on your network traffic or CPU utilization or parameter that you set. It launches instances from the launch template.
Note: we need to set up an Auto Scaling group 
Give a name to your ASG. E.g  ASG-frontend . And select the launch template that we have created for frontend (e.g template-frontend-server ) in the launch template field. And click on the next button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/c43e3582-da09-4e2b-bf2c-c7ed08fa4e55)
In the network field, you have to choose VPC that we created earlier.  And in AZs and subnet filed choose pri-sub-3a and pri-sub-4b. these subnets we have created for frontend servers. And click on the next button

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/af3d08d4-83a1-4dd3-abd3-2172a49cb855)

On this page we need to attach ASG with ALB so select the Attach existing ALB option and select  TG that we have created for frontend e.g  ALB-frontend-TG. And then scroll down and click on the NEXT button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/be8e2c5b-ea9a-4eec-a86a-14fa840422ef)

Here you can set the capacity and scaling policy but I’m keeping 1,1,1 to save cost but in real projects, it depends on the traffic.  Click on the NEXT->next->next-> and create ASG button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/0402414a-f8f3-42d0-a7c8-d74262d7b634)
Let's set up ASG for the backend
Give a name to your ASG. E.g ASG-backend. And select the launch template that we have created for the backend (e.g template-backend-server ) in the launch template field. And click on the next button.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/5a6fe405-19e3-4ddf-b4d7-32430c1fede5)

In the network field, you have to choose VPC that we created earlier.  And in AZ and subnet field choose pri-sub-5a and pri-sub-6b. these subnets we have created for backend servers. And click on the next button.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/8b31ddf4-c2bf-44fe-b048-81ef09a45dcb)

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/58cdb54d-3a84-40bc-a360-da6967d95d88)
Here you can set the capacity and scaling policy but I’m keeping 1,1,1 to save cost but in real projects, it depends on the traffic.  Click on the NEXT->next->next-> and create ASG button.
Now, We have two ASGs, ASG-frontend will launch frontend servers and ASG-backend will launch backend servers. we have successfully set up ASG 
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/d21cdfb6-9288-4e1d-ae75-26917ad1deea)

I hope you successfully completed the given step.

Now before we go further one more thing we need to do. We need to initialize our database and need to create some tables. But we can’t access the RDS instance or backend server directly coz they are in a private subnet and the security group won’t let us login into it. So we need to launch an instance in the same VPC but in the public subnet that instance is called bastion host or jump-server. And through that instance, we will log in to the backend server, and from the backend server we will initialize our database.
Click on the instance button on the left panel and click on the launch instance button in the top right corner. Please terminate those temp-servers if you haven't
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/481b49d3-8004-4100-a65d-bb3bfcf992d9)

Launch a instance and give  name to the instance (bastion-jump-server). Select Ubuntu as OS, instance typet2.micro, and select Key pair. In all the instance and launch template we have used only one key so it will be easy to login in any instance. And then click on the Edit button of the Network setting.
In the network setting select VPC that we have created and in the subnet select pub-sub-1a, you can select any public subnet from the VPC. and then select security group. We already have a security group with the name bastion-jump-server-sg and click on the launch instance.

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/adce1991-68d2-415e-953e-e5122fbd715d)
Once the instance becomes healthy, we can SSH into it. so select the instance and copy its public IP
Run this command

scp -i <name_of_your_key>.pem <name_of_your_key>.pem ubuntu@<Public_IP_add_of_instance>:/home/ubuntu/key.pem
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/5a2e8c62-42ef-4c4b-93bd-5646855a06c3)

The above command will copy our login secret key file into the bastion host.

Now type the below command to login into the Bastion host. And copy the public IP of the Bastion hos
ssh -i <name_of_your_key>.pem ubuntu@<Public_IP_add_of_instance>
Hit the below command to change the permission of the key.pem file

chmod 400 key.pem

Now we want login into the backend server so select backend server and copy its private IP address. You can identify the backend server by the security group attached to the instance.

Type the below command to log in to the backend server.

ssh -i key.pem ubuntu@<Private_IP_add_backend_server>
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/96e11ddb-2e4c-4f45-b60e-1921f3169161)
Now we are logged in inside the backend server.run this command
 cd Three-tier_code/backend/
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/ed3a2d21-27e8-4384-9bab-6d45ae51ff79)
We need to install one package type below the command

sudo apt install mysql-server -y
And type the below command to initialize the database.

mysql -h book.rds.com -u <user_name_of_rds> -p<password_of_rds> test < test.sql
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/89d398a4-cb25-4361-b07e-a36f33edd18e)

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/4eb6aca3-d294-46c0-8232-d593131e3479)



copy your fronted load blancer hit on browser 

![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/eefa6602-390e-4b46-8248-d4e2b719327a)


![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/be228f56-3cda-4386-aa09-45255be68745)
























































