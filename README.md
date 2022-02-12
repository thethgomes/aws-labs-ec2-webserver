# aws-labs-ec2-webserver-ELB
### OBJECTIVE
Creating an EC2 WebServer instance running with Load Balancer High Availability on AWS Infrastructure.

#### 1 - Creating EC2 Instances - WebServer

**1.Creating Web Server in a Public Subnet on Availabiliy Zone A**
To create a EC2 Web Server instance click Search bar in AWS console and type "EC2" then click on EC2.
on left panel find Instances menu then selec Instances and start creating a new instance clicking on **Launch instances** button.
Select first option for _Amazon Linux 2 AMI_ instance:

![image](https://user-images.githubusercontent.com/48591555/153715787-d2070156-1872-4073-9526-c4cb4c141a3e.png)

Select: _t2.micro_ for Instance Type then click on "**Next"Configure Instance details**":

![image](https://user-images.githubusercontent.com/48591555/153715850-2dfad674-579b-4527-bf3a-34c7bc2aadb2.png)

Set following options for Network 

**Network** : **vpc-prod1**
**Subnet** : **Public-A**

![image](https://user-images.githubusercontent.com/48591555/153716157-3a778a89-9650-486f-b27f-a19df49774e4.png)


Add following **"User data**"to build up an apache web server: 

```markdown
#!/bin/bash
# install httpd
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Web Server PROD from $(hostname -f)</h1>" > /var/www/html/index.html
```
![image](https://user-images.githubusercontent.com/48591555/153716296-1a23b65d-eb00-4a36-9aa3-e9343f358c29.png)

Click on **Next Add Storage**, there is no need to change then click on **Next Add Tags"**
Click on **Add Tag** and add following Tags:
![image](https://user-images.githubusercontent.com/48591555/153727750-5c5d5d02-64b5-41fc-901e-6d556af7dae0.png)

reate Following Security Group for the Web-Server
![image](https://user-images.githubusercontent.com/48591555/153716542-79cd120c-a357-4dac-9e87-b70c07f88bda.png)

Create a New-key pair for web-server.

**2.Creating Web Server in a Public Subnet on Availabiliy Zone B**
To create a EC2 Web Server instance click Search bar in AWS console and type "EC2" then click on EC2.
on left panel find Instances menu then selec Instances and start creating a new instance clicking on **Launch instances** button.
Select first option for _Amazon Linux 2 AMI_ instance:

![image](https://user-images.githubusercontent.com/48591555/153715787-d2070156-1872-4073-9526-c4cb4c141a3e.png)

Select: _t2.micro_ for Instance Type then click on "**Next"Configure Instance details**":

![image](https://user-images.githubusercontent.com/48591555/153715850-2dfad674-579b-4527-bf3a-34c7bc2aadb2.png)

Set following options for Network 

**Network** : **vpc-prod1**
**Subnet** : **Public-B**

![image](https://user-images.githubusercontent.com/48591555/153716157-3a778a89-9650-486f-b27f-a19df49774e4.png)


Add following **"User data**"to build up an apache web server: 

```markdown
#!/bin/bash
# install httpd
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Web Server PROD from $(hostname -f)</h1>" > /var/www/html/index.html
```
![image](https://user-images.githubusercontent.com/48591555/153716296-1a23b65d-eb00-4a36-9aa3-e9343f358c29.png)

Click on **Next Add Storage**, there is no need to change then click on **Next Add Tags"**
Click on **Add Tag** and add following Tags:

![image](https://user-images.githubusercontent.com/48591555/153727846-4713577a-b3a8-49d5-bfa0-0867459848a4.png)

Click on **"Next: Configure Security Group"** and **"Select an existing security group" selecting _SG-WebServer_ security group then Click on **"Review and Launch**":

![image](https://user-images.githubusercontent.com/48591555/153727900-22900bd0-de5c-433f-93f9-4e575b3c0c10.png)
Selecting precious key and click on **"Launch Instances"**:

#### 2 - Creating APP Load Balancers

Click on Load Balancing > Load Balancers then click on **"Create Load Balancer"** button:

Select **Aplication Load Balancer** 

![image](https://user-images.githubusercontent.com/48591555/153728082-1897e3e3-c784-4af1-8e70-69feb084d0a5.png)

Provide a name to the Load Balancer (_lb-prod01_):

![image](https://user-images.githubusercontent.com/48591555/153728168-b2462c8a-4e7b-4a96-9cff-c5b4ab33be11.png)

Select _us-east-1a_ and _us-east-1b_ Availability Zones:

![image](https://user-images.githubusercontent.com/48591555/153728205-30e4fa64-a3f8-4d4c-8124-606561af4965.png)

Define Listener ports through port 80 (HTTP) and Create a Target group clicking on  _Create target group_ :

![image](https://user-images.githubusercontent.com/48591555/153728236-866e7b44-c9d0-415f-9479-a485ea8068f4.png)

Specify Target group details:
![image](https://user-images.githubusercontent.com/48591555/153728257-ec7ffa78-cd15-4486-8105-8ed7eed5920c.png)

Provide a Target Group Name and select your target VPC:

![image](https://user-images.githubusercontent.com/48591555/153728354-eba3c5ed-d316-428c-833c-901fe58f451e.png)

Set Advanced Health ckeck settings:

![image](https://user-images.githubusercontent.com/48591555/153728434-c22a1c2b-9e70-45c2-92e5-658dde72bc1c.png)

Register targets:

![image](https://user-images.githubusercontent.com/48591555/153728462-f41e7576-4897-4331-b32e-854ad46de59b.png)

Click on Register targets:
![image](https://user-images.githubusercontent.com/48591555/153728536-10ad86ff-68c7-46f3-870a-c968fcd55cba.png)

select targets and click on **Include as pending below** : 
![image](https://user-images.githubusercontent.com/48591555/153728558-8a10e439-fb0e-4043-86d7-319366cda5c3.png)

click on Register pending targets:
![image](https://user-images.githubusercontent.com/48591555/153728570-f0f97b63-18bd-4db8-ae76-b5ea3ee5da5d.png)

click on Listeners and routing and select created target group then click on **Create load balancer**
![image](https://user-images.githubusercontent.com/48591555/15372716-0f2ef7bc-034b-4179-9b18-295387b6d6ad.png)

you may reach your Created load balancer:

![image](https://user-images.githubusercontent.com/48591555/153728748-df1de301-c9a7-47c2-8489-e96a63e57800.png)


Create Load balancer Security Group:
![image](https://user-images.githubusercontent.com/48591555/153728888-72783e11-bf1c-4860-a432-d12246850cf5.png)

Attach LoadBalancer Security Group for thew Load Balancer:

![image](https://user-images.githubusercontent.com/48591555/153728918-4069fd74-d6f5-4040-8b05-5aec791271af.png)

![image](https://user-images.githubusercontent.com/48591555/153728924-a5244eba-bcfd-48e9-87a3-ebee2452a753.png)

#####Testing Load Balancer:

Get load Balancer DNS name and paste into web browser:

![image](https://user-images.githubusercontent.com/48591555/153728971-469b13a6-0c64-4f82-83ea-65a647c84584.png)

![image](https://user-images.githubusercontent.com/48591555/153728980-1e95e9c8-a21b-44b9-8472-8d7dc94da145.png)

To check load balancer effectivenes you need to shutdown at least one of 2 virtual machines set for the load balancer.




