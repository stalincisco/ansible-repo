   # Ansible Notes
  
  ## 1) Step 1: Installing Python3
      
      $ su -
      
      $ sudo dnf update
      
      $ sudo dnf install python3
    
    **To verify that indeed you have python3 installed, run the command.**
    
      $ python3 -V

## Step 2: Installing PIP – The Python Package Installer

Pip is a Python’s package manager, which is also comes preinstalled, but again, in case Pip is missing on your system, install it using the command.

      $ sudo dnf install python3-pip

## Step 3: Installing the Ansible Automation Tool

      $ pip3 install ansible --user

To check the version of Ansible, run.

     $ ansible --version

Perfect! As you can see, the version of Ansible installed is Ansible 2.8.5.

## Step 4: Testing the Ansible Automation Tool

To test ansible, first ensure that ssh is up and running.
    
     $ sudo systemctl status sshd


## Step 3:  Modify hostname to controller, target1, target2

     $ sudo vi /etc/hostname  

Change hostname to ansiblecontroller

     $ sudo vi /etc/hosts

Change hosts to ansiblecontroller
127.0.0.1 localhost ansiblecontroller
::1       localhost ansiblecontroller

## Test Project
     
     $ mkdir test-projects
## ssh to all the 2 target  

Under test project folder create inventory.txt

    $ cat  > inventory.txt

Target2 ansible_host=192.168.1.15 ansible_ssh_pass=osboxes.org
Ctl+c to save

    $ cat inventory.txt
    
    $ansible target2 -m ping -I inventory.txt 

    $ sudo vi /etc/ansible/ansible.cfg 

And remove # before host_key_checking = False

    $ ansible all -m ping -i inventory.txt

    $ mkdir project-1

    Copy inventory.txt under project-1 folder
    
    $ vi playbook-pingtest.yaml

  http://www.yamllint.com/




  $ apm install linter-js-yaml

  $ ansible playbook playbook-pingtest.yaml -i inventory.txt
  
###Install Visual Studio Code or autom IDE

**Switch to the root user.**
      
     $ su -
    
**Download and import the Microsoft signing GPG key using the curl command.**

     $sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   
 **Now, add the Visual Studio Code repository to your system.**

$sudo nano /etc/yum.repos.d/vscode.repo

**Paste the following content to enable the VS Code repository:**

	[code]
	name=Visual Studio Code
	baseurl=https://packages.microsoft.com/yumrepos/vscode
	enabled=1
	gpgcheck=1
	gpgkey=https://packages.microsoft.com/keys/microsoft.asc

**Install Visual Studio Code**

    $ yum check-update
    $ sudo dnf install code
  
**Start Visual Studio Code**
  
    $ code

**Update Visual Studio Code**

    $ yum update code
  

