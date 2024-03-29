# pfSense

### Step 1. Create a VPC
Name is pfSense-firewall
Ipv4 CIDR is 10.1.0.0/16
Everything else will stay at default 
Click create

### Step 2. Create two subnets
VPC ID will be VPC we just created
Subnet 1: Name is internal-private
Ipv4 CIDR 10.1.1.0/24
Add Subnet 2: Name is external-public
Ipv4 CIDR 10.1.2.0/24
Click Create

### Step 3. Create Internet Gateway
Name is pfSense
Attach to VPC by pressing Action Button then Attach to VPC (Choose VPC we created)
Click attach

### Step 4. Create a EC2 Instance 
Click Launch Instance 
Name pfSense
Choose AMI: From search type in pfsense and select (You will have to pay for this AMI)
Size is already chosen for you
Create a Key pair: named pfsense (Use RSA and .pem file)
Go to Network settings and click edit
Choose VPC we created 
Subnet will be the external-private
Enable Auto -assign public IP (we can always change later)
Keep Create security group 
Inbound rules are already created from AMI
Launch Instance 

### Step 5. Create Network interface
Name is pfSense-Internal
Connect to internal-private subnet
Attach security group to the NI by pressing Actions, Attach, and select Instance we created
Press Attach

### Step 6. Create Route Tables
Go to the VPC and select the box next to it
Go to details and click on the Main route table number
Click the box next to it, then go to Routes
Click Edit Routes , then Add route: Destination 0.0.0.0/0, Target Internet gateway (Choose the Instance we created)
Click save
Go to Subnet associations and click Edit subnet associations
Choose the external-public subnet and click save
Click route table
Name internalroutes and choose VPC
Click create
Go to Edit routes and Add route
Destination 0.0.0.0/0, Target Internet Gateway (pfSense)
Save route
Go to Subnet Associations,  Edit subnet associations, choose internal-private and click save

### Step 7.
Go to the EC2 instance (pfSense), click on Networking
Scroll to Network interface, choose pfsense-internal
Go to Actions, Change source/dest.check and Disable.

### Step 8. Go to pfSense dashboard
Open a new tab and use the public IP address
You will need to enter the username and password.
User is admin. To find password go back to the Instance, Click Actions, then Monitor and troubleshoot, then Get system log.
Password will be at the top. Now copy it
Enter the Username and password
Click next until you get to the admin password page and use same password
Click finish
### Step 9. Configure
Go to Interfaces then Assignments
Add your LAN to the interface

### Step 10. Ping to test connectivity 
Do to Diagnostics and ping 



# Kali Linux

### Create EC2 instance
Navigate to the EC2 page in the AWS Services Console.

Name and tags
KALI-mr-rbt

Application and OS Images (Amazon Machine Image) 
Search  ‘Kali’  in the dashboard.
Click the  ‘AWS Marketplace AMIs’  tab.
The first result at top should be  ‘Ver Kali Linux 2023.4’.
Click on it, and click the  ‘Subscribe Now’  button on the following page.

Instance type
Back on the EC2 instance launch page, the minimum requirement for this Kali AMI is a:
Instance Type:  t2.medium

Each EC2 instance using the instance type incurs a charge of:
$0.046/Hr

This instance type is not eligible for our AWS free tier, but that’s the way the cookie crumbles.

After choosing the minumum Instance type, scroll down to  ‘Key pair (login)’.
Create new key pair
Key pair name:  KALI-mr-rbt-KEY-PAIR
Key pair type:  RSA
Private key file format:  .pem

Click  ‘Create key pair’  button.
This will create and download your key pair, which you’ll find in the Downloads folder by default if using Chrome browser.

Network Settings
VPC:  Mr-Robot-VPC
Subnet:  mr-rbt-private-subnet
Auto-assign public IP:  Enable

Security Group name:
Kali Linux-Kali Linux 2023.4-AutogenByAWSMP--2

Inbound  Security Group Rules
Type:  ssh
Protocol:  TCP
Port range:  22
Source type:  Anywhere

Configure Storage
The AWS free tier allows up to 30 GB of General Purpose SSD or Magnetic storage. The default is 12 GB gp2, so let’s take advantage and manually change the size and throughput quality of volume!
1x:  30 GiB  gp3

### Connect to Kali instance
Check the box for the KALI instance you’ve just created, and click on the  ‘Connect’  tab.

Click on the ‘copy to clipboard’ icon next to the chmod command on number 3.  This command will secure the key-pair file you downloaded earlier to 
Open your host machine’s terminal window.
Navigate to the Downloads folder.
Paste the command:
$  chmod 400 "KALI-mr-rbt-KEY-PAIR.pem"

This command has secured the key-pair file you downloaded earlier to only provide read permissions to root user, and restricts any other user of any permissions.

Go back to the  ‘Connect to instance’  page, and copy the ssh command in the provided ‘Example’ at bottom of page.
Paste it into your host machine’s terminal.
$  ssh -i "KALI-mr-rbt-KEY-PAIR.pem" root@54.227.178.232

When prompted with:
‘Are you sure you want to continue connecting (yes/no/[fingerprint])?’
...type yes:
$  yes

You should see the message:
‘Warning: Permanently added '54.227.178.232' (ED25519) to the list of known hosts.’

You’ve just added the public key to your system’s list of known hosts. Whenever you connect to this EC2 instance, the server will present it’s SSH key, and your host system will check in the  ‘known_hosts’  file for a matching key. If it matches, it is safe and you can connect.

You should now be connected to the Kali instance!

### Update Kali instance
Run:
$  sudo apt update
$  sudo apt upgrade
...to update the debian dependencies.


# Metasploit

### Create Ubuntu instance
Name:  METASPLOIT-ubuntu-mr-rbt
Key pair name:  METASPLOIT-ubuntu-mr-rbt-KEY-PAIR

Create an EC2 instance with the Quick Start Ubuntu AMI.

Give it the same options as the KALI instance.
Launch the instance.

You’ll be directed to the EC2 Instances page.

### Connect to Ubuntu instance
Check the box for the METASPLOIT instance you’ve just created, and click on the  ‘Connect’  tab.

Click on the ‘copy to clipboard’ icon next to the chmod command on number 3.  This command will secure the key-pair file you downloaded earlier to 
Open your host machine’s terminal window.
Navigate to the Downloads folder.
Paste the command:
$  chmod 400 "METASPLOIT-ubuntu-mr-rbt-KEY-PAIR.pem"

This command has secured the key-pair file you downloaded earlier to only provide read permissions to root user, and restricts any other user of any permissions.

Go back to the  ‘Connect to instance’  page, and copy the ssh command in the provided ‘Example’ at bottom of page.
Paste it into your host machine’s terminal.

When prompted with:
‘Are you sure you want to continue connecting (yes/no/[fingerprint])?’
...type yes:
$  yes

You should see the message:
‘Warning: Permanently added '34.204.17.208' (ED25519) to the list of known hosts.’

You’ve just added the public key to your system’s list of known hosts. Whenever you connect to this EC2 instance, the server will present it’s SSH key, and your host system will check in the  ‘known_hosts’  file for a matching key. If it matches, it is safe and you can connect.

You should now be connected to the Ubuntu METASPLOIT instance!

### Update Ubuntu instance
Run:
$  sudo apt update
$  sudo apt upgrade
...to update the debian dependencies.

## Install Metasploit
Now install some initial dependencies:
$  sudo apt install ruby ruby-dev build-essential zlib1g zlib1g-dev libpq-dev libpcap-dev libsqlite3-dev

Clone the Metasploit github repository:
$  git clone https://github.com/rapid7/metasploit-framework.git
$  ls

You should now have a  ‘metasploit-framework’  directory in your current directory. Now change into that directory:
$  cd metasploit-framework

Install Ruby’s bundler package manager:
$  sudo gem install bundler

Install the Ruby packages for Metasploit:
$  sudo bundle install

Finish installing the rest of the packages for Metasploit that are Ruby dependencies:
$  sudo bundle install
$  ls

You should now see an abundant list of directories and files.
Check proper installation by executing the console for the Metasploit Framework:
$  ./metasploit-framework

Congrats! You’ve just run Metasploit!!


# Wazuh solution
The  ‘Wazuh manager’  instance will comprise the following Wazuh components:
* Wazuh Indexer
* Wazuh Server
* Wazuh Dashboard

The ‘Wazuh agent’ will be installed on the following endpoint:
* Metasploit




## Wazuh manager

I’d like to mention three options here:

1. We can install Wazuh on an EC2 server instance, and eventually operate it through the CLI. This would be the easiest installation since we would not need to go through hurdles in installing certain other requirements to be able to use a graphical user interface (GUI). AWS console doesn’t provide us with an easy way to connect to a GUI from MacOS or Linux. Using the Wazuh manager from the CLI can be daunting and unintuitive. We might also be missing out on potential features provided by Wazuh Dashboard.
2. We can use the AWS Linux 2 AMI, which comes with MATE desktop pre-installed. But this is optimized to work with other AWS services which we might not be using, and it is not as widely supported by technologies as other Linux distributions.
3. We can go through the longer and more complex installation process on an EC2 Ubuntu instance, with Gnome Desktop environment. We’ll need to install certain additional technologies to bridge the gap, but it will pay off in the end. 

Let’s go with Linux Ubuntu AMI!

### Create Ubuntu instance
Name:  WAZUH-manager-mr-rbt
Key pair name:  WAZUH-manager-mr-rbt-KEY-PAIR

Create an EC2 instance with the Quick Start Ubuntu AMI.

Give it the same options as the WAZUH instance.
Launch the instance.

You’ll be directed to the EC2 Instances page.

### Connect to Ubuntu instance
Check the box for the WAZUH instance you’ve just created, and click on the  ‘Connect’  tab.

Click on the ‘copy to clipboard’ icon next to the chmod command on number 3.  This command will secure the key-pair file you downloaded earlier to 
Open your host machine’s terminal window.
Navigate to the Downloads folder.
Paste the command:
$  chmod 400 "WAZUH-manager-mr-rbt-KEY-PAIR.pem"

This command has secured the key-pair file you downloaded earlier to only provide read permissions to root user, and restricts any other user of any permissions.

Go back to the  ‘Connect to instance’  page, and copy the ssh command in the provided ‘Example’ at bottom of page.
Paste it into your host machine’s terminal.

When prompted with:
‘Are you sure you want to continue connecting (yes/no/[fingerprint])?’
...type yes:
$  yes

You should see the message:
‘Warning: Permanently added '34.234.97.108' (ED25519) to the list of known hosts.’

You’ve just added the public key to your system’s list of known hosts. Whenever you connect to this EC2 instance, the server will present it’s SSH key, and your host system will check in the  ‘known_hosts’  file for a matching key. If it matches, it is safe and you can connect.

You should now be connected to the Ubuntu WAZUH instance!

### Update Ubuntu instance
Run:
$  sudo apt update
$  sudo apt upgrade
...to update the debian dependencies.


Great! We now have an EC2 instance dedicated to our Wazuh manager. But now we need to give it a GUI. 

LXDE stands for Light-weight X11 Desktop Environment. It’s great for cloud-based servers.

Run the install command:
$  sudo apt-get install lxde

When prompted to continue, type  ‘y’

Since you’re pretty much installing an entire graphical environment, this installation will take a while (much longer than installing your average Linux server with no GUI).

When prompted, just keep hitting the  ‘Enter’  key on the defaults. 
Eventually, you’ll be back at the terminal prompt.

You’ve installed LXDE, but there is still no graphical interface to interact with. This is because we still need a few things in order to view the graphical application.

We need a VNC (Virtual Network Computing) client application on our host, and a VNC server application on our cloud instance. This is a graphical desktop-sharing system, which solves our problem for a graphical view through the RBF (Remote Buffer Protocol), which is a protocol much like SSH/ICMP/HTTP protocols. This protocol allows for remote access to Graphical User Interfaces. It works at the framebuffer level and is applicable to all windowing systems and applications, including Microsoft Windows, MacOS, X Window system, etc. The technologies in this guide are based on  ‘X Window’  windowing system.

We’ll be using x11vnc on the instance to accept the connection from our host’s VNC client.

## x11vnc
Let’s run the install command:
$  sudo apt install x11vnc

Create a password file for x11vnc:
$  sudo x11vnc -storepasswd
It’ll ask you to put in your password, and then a second time to verify it.
A  ‘.vnc’  directory has been created inside of your Home directory, with a  ‘passwd’ file inside of it:  ‘Password written to: /home/ubuntu/.vnc/passwd’

Let’s try running x11vnc manually:
$  sudo x11vnc -display :0 -auth /var/run/lightdm/root/:0 -forever -shared -rfbauth ~/.vnc/passwd
You should see some output messages about the service, including the IP and Port number at the bottom.

But instead of just running it manually every time, let’s create a systemd unit file so that we can manage x11vnc as a systemd service.
This allows us to start and stop x11vnc, as well as defining some other utilities while it runs.

### Create systemd unit file
Create a  ‘.service’  unit file in a nested  ‘systemd/system’  directory:
$  sudo nano /lib/systemd/system/x11vnc.service

```
[Unit]
Description=x11vnc service
After=display-manager.service network.target syslog.target

[Service]
Type=forking
ExecStart=/usr/bin/x11vnc -forever -display :0 -auth guess -passwd password
ExecStop=/usr/bin/killall x11vnc
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Exit nano, and write/save the file with  ‘ctrl + x’ ,  ‘y’ ,  and  ‘Enter’.

What does this file do?
[Unit]
‘Description’  provides a human-readable description of the service.
‘After’ specifies other systemd services that this unit depends on.
‘display-manager.service’,  ‘network.target’,  ‘syslog.target’  will be started, and then this unit will run after. (this property should be called  ‘Before’ instead)

[Service]
* ‘Type’ specifies the process startup type. We have set it to ‘forking’, which means it expects the main process of the service to exit once the daemonization is complete, and it starts monitoring the PID of the child process.
* ‘ExecStart’ defines the command to start the service. It runs  ‘/usr/bin/x11vnc’  with the following flags:
-forever  :  x11vnc will run indefinitely without it dying the way it normally would when we disconnect
-display :0  :  this is the display that will be captured (the first one)
-auth guess  :  attempts to guess the authority file
-passwd password  :  this sets the password file for authentication. Here we are setting the password as  ‘password’, but you can (and should) put in a different password
* ‘ExecStop’  specifies the command to stop the service. It kills all instances of x11vnc
* ‘Restart’  configures the conditions under which the service should be restarted. Here, it’s set to  ‘on-failure’,  meaning it will be restarted only if the main process exits with a non-zero exit code

[Install]
* ‘WantedBy’  specifies the target that this service is associated with. Here, it’s set to  ‘multi-user.target’,  which is a standard target that represents a system running in multi-user mode  (for example, all services are running, except for graphical user interface)


Essentially, the unit file does the following:
* Starts after some other services have been started / target points have been reached
* Forking, since x11vnc creates a child process from the -forever clause everytime something connects to it
* The ExecStart and ExecStop commands and Restart are commands that we would write in manually to start/stop/restart it
* We want it to be restarted on failure
* We also want the service to be started before the process reaches the multi-user target

### Use systemd unit file
Now that we’ve created the unit, you need to let systemd know that they exist. I believe it’s like a refresh:
$  sudo systemctl daemon-reload

Then, tell systemd to enable it:
$  sudo systemctl enable x11vnc.service

Now let’s start it:
$  sudo systemctl start x11vnc.service

You should see a hanging terminal.
Let’s check this by stopping it with  ‘ctrl + c’ , and then running:
$  sudo systemctl status x11vnc.service
This tells us that the service was indeed running, so you should have no worries the next time you start the process!
You should see the Port number again. Make a note of that number, to use next.

You’ve just installed, configured, and run x11vnc!

### Allow VNC access
Before you proceed, go back to the WAZUH EC2 instance’s Security Group and allow for VNC port traffic.
You’ll need that port number (the default port is 5900).
EC2 Instance page  ->  WAZUH-manager-mr-rbt  ->  Details panel  ->  Details  ->  Security groups  ->  
Click on  ‘Edit inbound rules’:
Source type:  Custom TCP
Protocol:  TCP
Port range:  5900
Source:  Anywhere-IPv4  ;  0.0.0.0/0
Description:  For VNC traffic

## TigerVNC
Now you’ll need to download and install a VNC desktop client on our host computer (in this case, it’s an m1 Mac).
You’ll be using TigerVNC for this. It started as a fork of TightVNC over a decade ago. The most notable difference between the two might be that TightVNC allows file transfers, and TigerVNC provides quality remote display functionality. You don't need a VNC to transfer files, as SCP protocol allows you to do this securely and easily. 

The quality option for viewing the Wazuh Dashboard would be TigerVNC.
If you have Homebrew installed, you can EASILY use its installer:
$  brew install tiger-vnc
Otherwise, you’ll first need to install Homebrew (it’s worth it! It makes it so much easier to download packages that might be tedious to install).

Execute TigerVNC with the command:
$  vncviewer

A small window should pop up, prompting the connection details.
Enter the instance's public IP address and port number, separated by a colon:
54.227.178.232:5900

A window should pop up, displaying the remote EC2 instance's LXDE GUI.
Congrats!

