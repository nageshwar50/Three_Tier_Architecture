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

##  Security Groups (SG)

Security groups are very essential part of the infrastructure. Because it can secure the resources in the cloud. SGs are a kind of firewall that allow or block incoming and outgoing traffic. SGs are applied to the resources like ALB, ec2, rds, etc. One resource can have more than one SG.
So let's first understand. How SG will be used in our architecture and how we are going to apply that. Please see the below image you will get all the ideas. Which resource depends on what. And what are the port numbers we need to allow etc..

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
And here our SG setups are complete now..

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

This page is similar to creating a database. In the AWS region select the region where you want to create the read replica. In my case, It is US East (N. Virginia) us-east-1.  Give a name to your read replica, and select all the necessary configurations that we did before while creating the database. For your reference, I have shown everything in the below images. You can check your read replica on the specified region’s RDS dashboard
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/2bd1fa75-2a1a-4c6d-bd39-4a1d7df42fcd)
## Note:
we can’t write anything into a read replica. It is just read-only database. So when a disaster happens we just have to promote read replica so that it becomes the primary database in that region.


## Route 53
Now we are going to utilize route 53 service and create two private hosted zone.  for north Virginia(us-east-1 Firstly, we are gonna create a hosted zone for us-east-1. Click on the Hosted Zones button on the left panel and click on the created hosted zone button on the top right corner.
![image](https://github.com/nageshwar50/Three_Tier_Architecture/assets/128671109/f569d641-1405-46b9-a012-db63ec036d00)

Give any domain name because anyhow it will be private hosted zone but it would be great if you give the name same as mine (rds.com). Please select the private hosted zone and Select the region. In my case, it is us-east-1. And then select VPC ID. Make sure you select VPC that we created earlier. Because this hosted zone will resolve the record only in specified VPC.  and then click on the Create hosted zone
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

here is my wildcard ssl which i have imported on aws 
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
you can use userdatata.sh code ..

we have successfully launched temp-frontend-server. so now let’s launch a temporary backend server.  give a name to your instance (temp-backend-server).  select ubuntu as the operating system. And select t2.mirco as instance type. Here we don’t have to create a new key, we can utilize the previous key that we have created while launching the frontend instance.
Scroll down to the bottom of the page, and copy the bash script that I have given backuserd.sh . and paste it in the USER-DATA text box. This bash scripting installs some packages so that we don’t have to install them manually. And click on the launch instance.

Please wait for 5-8 minutes so that the instance comes in a running state. and then we will utilize instances for further steps.
Select temp-frontend-server. and copy the IP address of the instance. Now open the Terminal where you have downloaded your YOUR_KEY.pem file. And type the command.
ssh -i <name_of_key>.pem ubuntu@<Public_IP_add_of_Instance> like
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









