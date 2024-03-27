# Kali Linux
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


Connect to Kali instance
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

Update Kali instance
Run:
$  sudo apt update
$  sudo apt upgrade
...to update the debian dependencies.






# Metasploit

Create Ubuntu instance
Name:  METASPLOIT-ubuntu-mr-rbt
Key pair name:  METASPLOIT-ubuntu-mr-rbt-KEY-PAIR

Create an EC2 instance with the Quick Start Ubuntu AMI.

Give it the same options as the KALI instance.
Launch the instance.

You’ll be directed to the EC2 Instances page.

Connect to Ubuntu instance
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

Update Ubuntu instance
Run:
$  sudo apt update
$  sudo apt upgrade
...to update the debian dependencies.

Install Metasploit
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