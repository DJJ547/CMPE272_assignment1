# CMPE272_assignment1
HW #1 - Ansible
- Utilize the Dcloud DevNet Express Data Center v2 lab, or deploy your own three (3) Virtual Machines
- Configure Ansible server on VM 1 to deploy a webserver to VM2 and VM3 on port 8080 that displays the message: “Hello World from SJSU-X”, where X is 1 or 2 depending on which webserver.
- Include in the Ansible playbook, plays to deploy and un-deploy all the webserver resources
- Due 9/11 (Sunday) at 11:59PM
- Submit a Word document via Canvas, with screenshots showing your work, and all ansible code/scripts via github

## Useful YouTube turtorial:
### 1. [What Is Ansible? | How Ansible Works? | Ansible Playbook Tutorial For Beginners | DevOps|Simplilearn](https://www.youtube.com/watch?v=wgQ3rHFTM4E)

### 2. [Ansible Installation & Configuration on AWS | Install & Configure Ansible on EC2 | Intellipaat](https://www.youtube.com/watch?v=Km3BCQnV6sw)

### 3. [Ansible Playbook Explained | Ansible Playbook Tutorial | How to Create Ansible Playbook](https://www.youtube.com/watch?v=CXP-5XkBvWI)

# Step by step guide #

## Step 1: Launch three instances (create three virtual machines) on [AWS EC2]( https://console.aws.amazon.com/ec2/) 
  - One for Ansible server and two for child server
  - For Application & OS select "Ubuntu"
  - For Key Pair use "create new key pair" and have the .pem files (only do this once, use this file on all three VMs) store somewhere for now
## Step 2: Go to your Visual Studio Code
  - click on 'Extension' search for "Remote-SSH" and install it
  - click on 'Remote Explorer', make sure in the drop down menu 'Remotes(Tunnels/SSH)' is selected
  - right click 'SSH', select 'open SSH config file'
  - In pop-out window select 'C:<your_path>\.ssh\config'
  - In the config file, copy & paste and edit this block of code:
    ```linguist
    Host <whatever name you want to name your ansible server>
        HostName <the ip address of your AWS ansible server>
        User ubuntu
        IdentityFile <path to your .pem key file, wrap with double quote if any folder or file name contains space, else not>

    Host <whatever name you want to name your child server 1>
        HostName <the ip address of your AWS child server 1>
        User ubuntu
        IdentityFile <path to your .pem key file, wrap with double quote if any folder or file name contains space, else not>

    Host <whatever name you want to name your child server 2>
        HostName <the ip address of your AWS child server 2>
        User ubuntu
        IdentityFile <path to your .pem key file, wrap with double quote if any folder or file name contains space, else not>
    ```
    save it, then you will see three host ip appear on the left
  - hit F1, type in 'Remore-SSH: Connect to Host' to connect to each of them
  - a pop up window with drop down, select 'linux' (then maybe another window asking about key fingerprint, select yes)
## Step 3: Now that you have a new console of VSC connnected to the server
  ### For ansible server
  - go to Terminal -> new terminal, first update server with this command
    ```linguist
    sudo apt-get update
    ```
  - then update local repo with offical ansible project repo
    ```linguist
    sudo apt-add-repository -y ppa:ansible/ansible
    ```
  - update again
    ```linguist
    sudo apt-get update
    ```
  - install ansible
    ```linguist
    sudo apt-get install -y ansible
    ```
  - then install python 3 pip with this command
    ```linguist
    sudo apt install python3-pip -y
    ```
  ### For each child server
  - go to Terminal -> new terminal, first update server with this command
    ```linguist
    sudo apt-get update
    ```
  - then install python 3 pip with this command
    ```linguist
    sudo apt install python3-pip -y
    ```
## Step 4: Configure SSH keys
  - open your ansible server with VSC
  - terminal -> new terminal, enter
    ```linguist
    cd .ssh
    ```
  - generate ssh key with this command:
    ```linguist
    ssh-keygen
    ```
  - It will ask you to enter file in which to save the key, remember the path, hit 'enter', ask you to enter passphrase, hit "enter", passphrase again, hit "enter"
  - to copy the public key, read the file with this command:
    ```linguist
    cat id_rsa.pub
    ```
  - copy the whole thing (include two words at the beginning and at the end seperared with space)
  - open each of your child server with VSC, terminal -> new terminal, enter:
    ```linguist
    cd .ssh
    ```
  - enter this command to edit the 'authorized_keys' file:
    ```linguist
    sudo nano authorized_keys
    ```
  - paste the same public key you copied to the next line
  - press ctrl + X to exit, hit 'Y', then hit 'enter'
## Step 5: Configure Ansible on the ansible server
  - open your ansible server with VSC
  - terminal -> new terminal, use 'cd ..' and 'cd <folder name>' to go to your 'ansible' folder
  - edit the 'hosts' file with this command:
    ```linguist
    sudo nano hosts
    ```
  - add this block of code to your hosts:
    ```linguist
    [production]
    child1 ansible_ssh_host=<child 1 server ip address>
    child2 ansible_ssh_host=<child 2 server ip address>
    ```
  - press ctrl + X to exit, hit 'Y', then hit 'enter'
  - now to test the ansible connection, you can enter this command:
    ```linguist
    ansible -m ping all
    ```
    and you will see both child servers respond with 'pong'
## Step 6: create ansible playbook
  - open your ansible server with VSC
  - file -> open folder, under your '/home/ubuntu/', hit ok
  - on the left panel showing all files, create a folder named 'playbooks' to store all your ansible playbooks
  - create a new file in your 'playbooks' and name it 'message.html', and copy & paste this in it:
    ```linguist
    <h1>Hello World from SJSU 1</h1>
    ```
  - place all four playbooks and the 'message_site.conf' in this github repo into your 'playbooks' folder
  - modify line 2 in every playbook, replace the 'node#' after '- hosts: ' with 'child1' or 'child2'
  - On your amazon EC2 instances page, click on your child server 1, go to 'Security', click on the link below 'Security groups', in the new page select the one and only security group rule, click on 'Edit inbound rules', then click 'Add rule', in the new row created just change the 'port range' to 8080, change 'Source' to 'Anywhere-IPv4', then hit 'Save rules'. Now that you have opened both your child servers port 8080
## Step 7: run your ansible playbook
  - open your ansible server with VSC
  - terminal -> new terminal, go into playbooks folder with:
    ```linguist
    cd playbooks
    ```
  - run this command to run deploy-node1.yaml:
    ```linguist
    ansible-playbook deploy-node1.yaml
    ```
  - run this command to run deploy-node2.yaml:
    ```linguist
    ansible-playbook deploy-node2.yaml
    ```
  - now open your browser and go to <child1's IP address>:8080, you will see message 'Hello World from SJSU-1' displayed, same with child2
### Now let's undeploy both server
  - run this command to run undeploy-node1.yaml:
    ```linguist
    ansible-playbook undeploy-node1.yaml
    ```
  - run this command to run undeploy-node2.yaml:
    ```linguist
    ansible-playbook undeploy-node2.yaml
    ```
