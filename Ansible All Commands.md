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


## Step 5:  Modify hostname to controller, target1, target2

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

-

   name: Test Connectivity to the target servers
   hosts: all
   taskts:
     - name: ping test 
       ping:
      
   **save the file**

    $ ansible -playbook playbook-pingtest.yaml -i inventory.txt

http://www.yamllint.com/

## now install VScode IDE or Atom IDE on your local machine and create inventory.txt and playbook-pingtest.yaml

**install linter-js-yaml**

**install atom remote sunc**


**create a file under tmp folder**

    $ cat > /tmp/test-file.txt
  Hello! This is a sample contecnt in test file
  ctl+c
   
    $ cat /tmp/test-file.txt
  Hello! This is a sample contecnt in test file

go to exercise-1-copy-file folder and execute

make sure the test-file.txt dosent exist in target1 and 2 severs.

    $ ansible-playbook playbook-copyfile.yaml -i inventory.txt
    $ ansible playbook playbook-pingtest.yaml -i inventory.txt


## Module Coding Exercise

### Ansible Modules

    1) Update the playbook with a play to Execute a script on all web server nodes. The script is located at /tmp/install_script.sh

    Use the Script module from https://docs.ansible.com/ansible/latest/collections/ansible/builtin/script_module.html

    -
        name: 'Execute a script on all web server nodes'
        hosts: web_nodes
        tasks:
            -
               name: 'Execute a script on all web server nodes'
               script: /tmp/install_script.sh


    2) Update the playbook to add a new task to start httpd services on all web nodes
    
    Use the Service module from https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html

    -
      name: 'Execute a script on all web server nodes'
      hosts: web_nodes
      tasks:
          -
             name: 'Execute a script'
             script: /tmp/install_script.sh
          -
            name: 'Start httpd service'
            service: 'name=httpd state=started'
	    

    3) Update the playbook to add a new task in the beginning to add an entry into /etc/resolv.conf file for hosts. The line to be   
       added is nameserver 10.1.250.10

    **Note:** The new task must be executed first, so place it accordingly.

    Use the Lineinfile module from https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html


     -
       name: 'Execute a script on all web server nodes'
       hosts: web_nodes
       tasks:
           -
              name: 'Execute a script'
              script: /tmp/install_script.sh
           -
              name: 'Start httpd service'
              service:
                 name: httpd
                 state: present

    4) Update the playbook to add a new task at second position (right after adding entry to resolv.conf) to create a new web user.

    Use the user module for this. User details to be used are given below:
    Username: web_user
    uid: 1040
    group: developers

    user Module from https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html


     -
     name: 'Execute a script on all web server nodes and start httpd service'
     hosts: web_nodes
     tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
		
## Create a Playbook1.yaml with above instructions. 


------------------------------------------------------------------------------------------------------------------------
## Sample Inventory File

## Web Servers
    sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
    sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
    web_node1 ansible_host=web01.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass
    web_node2 ansible_host=web02.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass
    web_node3 ansible_host=web03.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

    [db_nodes]
    sql_db1
    sql_db2

    [web_nodes]
    web_node1
    web_node2
    web_node3

    [all_nodes:children]
    db_nodes
    web_nodes

------------------------------------------------------------------------------------------------------------------



## Variables Coding Exercise

### Ansible Variables


    1) The playbook is used to update name server entry into resolv.conf file on localhost. The name server information is also 
       updated in the inventory file as a variable nameserver_ip. Refer to the inventory file.

    Replace the ip of the name server in this playbook to use the value from the inventory file, so that in the future if you had to   
    make any changes you simply have to update the inventory file.


  -
        name: 'Update nameserver entry into resolv.conf file on localhost'
        hosts: localhost

        tasks:
        -
            name: 'Update nameserver entry into resolv.conf file'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'

    2) We have added a new task to disable SNMP port in the playbook. However the port is hardcoded in the playbook. Update the 
       inventory file to add a new variable snmp_port and assign the value used here. Then update the playbook to use value from the 
       variable.


        -
            name: 'Update nameserver entry into resolv.conf file on localhost'
            hosts: localhost
            tasks:
        -
             name: 'Update nameserver entry into resolv.conf file'
             lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver {{ nameserver_ip }}'
        -
            name: 'Disable SNMP Port'
            firewalld:
                port: 160-161
                permanent: true
                state: disabled
		
**Remember to use curly braces around the variable name.**

    3) We are printing some personal information to the screen. We would like to move the car_model, country_name and title to a  
       variable defined at the play level.

       Create three new variables (car_model, country_name and title) under the play and move the values over. Use the variables in 
       the task. 

      -
        name: 'Update nameserver entry into resolv.conf file on localhost'
        hosts: localhost
        tasks:
        -
            name: 'Print my car model'
            command: 'echo "My car''s model is BMW M3"'
        -
            name: 'Print my country'
            command: 'echo "I live in the USA"'
        -
            name: 'Print my title'
            command: 'echo "I work as a Systems Engineer"'


### Sample Inventory File

    localhost ansible_connection=localhost nameserver_ip=10.1.250.10





# Appendix 

    $ apm install linter-js-yaml

 
  
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
  

