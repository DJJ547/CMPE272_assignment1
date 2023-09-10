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
  - For Key Pair use "create new key pair" and have the .pem files (total of three) store somewhere for now
## Step 2: Go to your Visual Studio Code
  - click on 'Extension' search for "Remote-SSH" and install it
  - click on 'Remote Explorer', make sure in the drop down menu 'Remotes(Tunnels/SSH)' is selected
  - right click 'SSH', select 'open SSH config file'
  - In pop-out window select 'C:<your_path>\.ssh\config'
  - In the config file, the format should be:
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
  - open command prompt on your local machine enter this command:
    ```linguist
    ssh-keygen
    ```
  - It will ask you to enter file in which to save the key, remember the path (usually "C:\Users\<your username>/.ssh/id_rsa"), then just hit 'enter', ask you to enter passphrase, just hit "enter", passphrase again, hit "enter"
  - to copy the public key, cd to the path with your local machine's windows terminal, open the file with this command:
    ```linguist
    cat id_rsa.pub
    ```
  - copy the whole thing (include the username at the end)
  - open each of your server with VSC, go to file -> open folder -> enter "/home/ubuntu/.ssh/" -> hit 'ok'
  - click the authorized_keys on the left to open it
  - paste your public key copy to the next line
  - file -> hit 'save'
## Step 5: Configure Ansible on the ansible server
  - open your ansible server with VSC
  - file -> open folder, enter '/etc/ansible/', click 'ok'
