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
Now we are going to create Internet Gateway also known as IGW.  it is responsible for communication between VPC, VPC's public subnet with the Internet. without IGW  we won't be able to communicate with the Internet. so let's create that. click on the  internet gateways button on the left panel. and then click on the Create Internet gateways button on the top right corner of the page.

