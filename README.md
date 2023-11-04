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

Note: RDS take 15-20 minute because it creates a database and then take a snapshot. So please have patience and wait for it to be ready












