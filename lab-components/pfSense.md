# pfSense

![pfSense logo](./assets/pfSense-logo.jpeg)


## Step 1. Create a VPC
Name is `pfSense-firewall`<br>
Ipv4 CIDR is `10.1.0.0/16`\
Everything else will stay at default.  
Click `create`

## Step 2. Create two subnets
VPC ID will be VPC we just created<br>
Subnet 1: Name is `internal-private`<br>
Ipv4 CIDR `10.1.1.0/24`<br>
Add Subnet 2: Name is `external-public`<br>
Ipv4 CIDR `10.1.2.0/24`<br>
Click `Create`<br>

## Step 3. Create Internet Gateway
Name is `pfSense`<br>
Attach to VPC by pressing `Action` button, then `Attach to VPC` (Choose VPC we created)<br>
Click `attach`

## Step 4. Create a EC2 Instance 
Click `Launch Instance`<br>
Name `pfSense`<br>
Choose AMI: From `search type` in pfsense and select (You will have to pay for this AMI)<br>
Size is already chosen for you.<br>
Create a Key pair: named `pfsense` (Use RSA and .pem file)<br>
Go to `Network settings` and click `edit`<br>
Choose VPC we created.<br>
Subnet will be the `external-private`<br>
Enable `Auto-assign public IP` (we can always change later)<br>
Keep `Create security group`<br>
Inbound rules are already created from AMI.<br>
`Launch Instance`

## Step 5. Create Network interface
Name is `pfSense-Internal`<br>
Connect to `internal-private` subnet<br>
Attach security group to the NI by pressing `Actions` > `Attach` > select Instance we created.<br>
Press `Attach`<br>

## Step 6. Create Route Tables
Go to the VPC and select the box next to it.<br>
Go to details and click on the `Main route` table number.<br>
Click the box next to it, then go to `Routes`<br>
Click `Edit Routes` , then Add route: `Destination 0.0.0.0/0`, `Target Internet gateway` (Choose the Instance we created)<br>
Click `save`<br>
Go to `Subnet associations` and click `Edit subnet associations`<br>
Choose the `external-public` subnet and click save.<br>
Click `route table`<br>
Name `internalroutes` and choose VPC<br>
Click `create`<br>
Go to `Edit routes` and `Add route`<br>
`Destination 0.0.0.0/0`, `Target Internet Gateway` (pfSense)<br>
`Save route`<br>
Go to `Subnet Associations` > `Edit subnet associations` > choose `internal-private` and click `save`

## Step 7.
Go to the EC2 instance (pfSense), click on `Networking`<br>
Scroll to `Network interface`, choose `pfsense-internal`<br>
Go to `Actions`, Change `source/dest.check` and `Disable`

## Step 8. Go to pfSense dashboard
Open a new tab and use the public IP address.<br>
You will need to enter the username and password.<br>
User is `admin`. To find password go back to the Instance, click `Actions`, then `Monitor and troubleshoot`, then `Get system log`<br>
Password will be at the top. Now copy it.<br>
Enter the Username and Password.<br>
Click next until you get to the `admin password` page and use same password.<br>
Click `finish`.

## Step 9. Configure
Go to `Interfaces` then `Assignments`.<br>
Add your LAN to the interface.

## Step 10. Ping to test connectivity 
Go to `Diagnostics` and ping.<br>
