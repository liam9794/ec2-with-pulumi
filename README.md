# Starting Your Own Minecraft Server Automatically
## _Uses Pulumi, AWS resources, and Ansible_

### [Bash Script Drawn Heavily From Amazon's Tutorial Found Here](https://aws.amazon.com/blogs/gametech/setting-up-a-minecraft-java-server-on-amazon-ec2/)

##### This walk through utilizes sfotware such as Pulumi (currently v3.117.0), AWS instances, and Ansible (currently v2.9.23)

## Spinning Up AWS Instance
1. First we will run the Pulumi scripts provided here in the GitHub. You must install Pulumi on your machine.
```Yum -y install pulumi```
Additionally, you must have an existing key file on AWS. The key pair must be named **MinecraftServerKey**. It is crucial to follow the name scheme, so that Pulumi is able to assign that existing key pair to the Instance it creates.
2. Next, in order for Pulumi to connect to your AWS account and create the necessary utilities, you must verify yourself via Pulumi. In the home directory of the user on your machine, you must alter the .aws/credentials file.
3. Start your AWS lab. Once it is started, (the light is green), click **AWS Details** in the top right of your lab, then next to AWS CLI click **Show**. Copy the entire credentials field, and paste it into your .aws/credentials file.
3. With Pulumi installed, your lab running, your credentials updated, and your MinecraftKeyPair existing in AWS, all you have to do is run:
```pulumi up```
and all the neccessary resources will be automatically created for you. Pulumi should output the IP of the Instance that was created. Make note of this, as this will be how we access the Instance both for Ansible and for playing Minecraft!

## Configuring The AWS Instance via Ansible
1. Now, we are going to run the Ansible playbook. The ansible playbook will connect to the newly created instance, transfer the Bash script that I've written, and run it. First, you must have Ansible installed on your machine. 
```Yum -y install ansible```
2. Now, let's configure our Ansible. It is important to note that you will need to know the path of your key associated with AWS. With you choice of text editor, open the /etc/ansible/hosts file. 
```sudo nano /etc/ansible/hosts```
On two consecutive lines, without leading hashtags (these are commented out lines), write the following and replace with your own information:
On the first line:
```[server]```
On the next consecutive line:
```XX.XX.XX.XX ansible_user=ec2-user ansible_ssh_private_key_file=~/path/to/MinecraftServerKey```
Be sure to replace ```XX.XX.XX.XX``` with the IP associated with your Instance. Replace the ```path/to/MinecraftServerKey``` with the path/location of your own key. For example:
```10.3.12.15 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/MinecraftServerKey```
3. Now we will run the playbook.yml. Execute the following command:
```ansible-playbook playbook.yml```
This command executes the playbook that I created, and the playbook is very simple. It transfers the bash script I created to the Instance, and executes it. The bash script accomplishes the same steps as all of Part 1, but without you the user having to do anything, or SSH into the Instance.

## Accessing Your Minecraft Server
First, give it a minute or two for the server to spin up and autostart. Then, open the Minecraft Java Edition. Click on **Multiplayer**, then click on **Direct Connection**. Paste the IP Address assoicated with the instance (the one Pulumi provided, and you used for Ansible) into the **Server Address** field, then click **Join Server**. Et voila! 

### Congratulations!
You have sucessfully created an AWS Instance using Pulumi, and configured it using Ansible (and a Bash file), and now you have a funcitoning Minecraft Server!
