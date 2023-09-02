# CMPE272_assignment1
HW #1 - Ansible
- Utilize the Dcloud DevNet Express Data Center v2 lab, or deploy your own three (3) Virtual Machines
- Configure Ansible server on VM 1 to deploy a webserver to VM2 and VM3 on port 8080 that displays the message: “Hello World from SJSU-X”, where X is 1 or 2 depending on which webserver.
- Include in the Ansible playbook, plays to deploy and un-deploy all the webserver resources
- Due 9/11 (Sunday) at 11:59PM
- Submit a Word document via Canvas, with screenshots showing your work, and all ansible code/scripts via github

## Install Ansible on Windows:
### 1.Install WSL:
[How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

### 2.Install Ansible through WSL(Ubuntu):
[How to install Ansible in Windows 11 WSL Windows Subsystem for Linux - Ansible install](https://www.youtube.com/watch?v=OhCbpGBOACs).

## Install Ansible on Mac:
### 1.Install packages below on the Server Machine
> $ sudo apt get install python yaml
> python jinja2 python paramiko python
> crypto python keyczar ansible
### 2.Install packages below on the Client Machines
> $ sudo apt get install python crypto
> python keyczar
