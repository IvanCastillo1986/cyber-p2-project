# Wazuh solution
The  **Wazuh manager**  instance will comprise the following Wazuh components:<br>
* Wazuh Indexer
* Wazuh Server
* Wazuh Dashboard

The  **Wazuh agent**  will be installed on the following endpoint:<br>
* Metasploit



## Wazuh manager
I’d like to mention four options here:

1. We can install Wazuh on an EC2 server instance, and eventually operate it through the CLI. This would be the easiest installation since we would not need to go through hurdles in installing certain other requirements to be able to use a graphical user interface (GUI). AWS console doesn’t provide us with an easy way to connect to a GUI from MacOS or Linux. Using the Wazuh manager from the CLI can be daunting and unintuitive. We might also be missing out on potential features provided by Wazuh Dashboard.
2. You can choose step 1, and just access the Wazuh dashboard from your graphical desktop host web browser.
3. We can use the AWS Linux 2 AMI, which comes with MATE desktop pre-installed. So you won’t need to run up your allowance of data from downloading a bulky GUI. But this is optimized to work with other AWS services which we might not be using, and it is not as widely supported by technologies as other Linux distributions.
4. We can go through the longer and more complex installation process on an EC2 Ubuntu instance, with LXDE Desktop environment. You’ll need to install certain additional technologies to bridge the gap.

## Two options for installing Wazuh manager
Here I list how to create an Ubuntu AMI LXDE instance, and another instance of Amazon Linux AMI.
I suggest going with the Amazon Linux instance.<br>
LXDE is a much older GUI environment for Ubuntu, and requires much more work, and perhaps troubleshooting since it's not as actively maintained now. Additionally, the VNC connection carries more security risks.


[Amazon Linux AMI with RDP connection](/Wazuh_manager_Amazon_Linux.md)
[Ubuntu with LXDE graphical interface](/Wazuh_manager_Ubuntu_LXDE.md)