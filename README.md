# Kode Kloud Ansible for the Absolute Beginners Course Notes
My notes from the Kode Kloud Ansible for the Absolute Beginners Course

Updated 2021-08-30

## Installing Ansible
1. On RHEL run
```
yum install -y ansible
```

## Testing ansible connectivity between controller and endpoints
1. Create a working directory for your lab project and then create inventory file - inventory.txt
```
target1 ansible_host=10.1.10.101 ansible_ssh_pass=Passw0rd!
target2 ansible_host=10.1.10.102 ansible_ssh_pass=Passw0rd!
```
2. Edit ansible.cfg in the /etc/ansible directory
```
sudo vi ansible.cfg
```
> Remove the comment from this line - host_key_checking = False from the ansible.cfg config file.
> 
> This not recommended for production, but the Kode Kloud folks have us do this for the local lab setup.

3. From the projects directory where you created the inventory.txt file run the following ansible command
```
ansible target1 -m ping -i inventory.txt
ansible target2 -m ping -i inventory.txt
```
> You should receive a successful ping

## Understanding Yaml

Example of key value pair in YAML
```
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chicken
```

Example of Array/Lists in Yaml
```
Fruits:
-   Orange
-   Apple
-   Banana

Vegetables:
-   Carrot
-   Cauliflower
-   Tomato
```

Example of Dictionary/Map
```
Banana:
    Calories: 105
    Fat: 0.4 g
    Carbs: 27g
     
Grapes:
    Calories: 62
    Fat: 0.3 g
    Carbs: 16 g
```

Key that you use the equal number of spaces in YAML. 

Dictionary is used to store properties of a particular object, like a car.

We want to store the name of six cars, we would use a list since a list of the same time of objects.

Dictionary is an unordered collection where as a list is a ordered list.

A hash # is ignored and considered a comment.

## Ansible Inventory

Uss ssh in Linux/Unix or Windows Powershell for connecting to remote systesm

Default inventory stored in /etc/ansible/hosts.  You can define inventor file in another working directory

sample Inventory file 
```
# Database Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Web Servers
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass

[db_nodes]
sql_db1
sql_db2

[web_nodes]
web_node1
web_node2
web_node3

[boston_nodes]
sql_db1
web_node1

[dallas_nodes]
sql_db2
web_node2
web_node3

[us_nodes:children]
boston_nodes
dallas_nodes
```
## Ansible Playbooks
Define what we want ansible do on the target endpoint.

Playbook is a single YAML file containing a set of ploys
- A play is a set of actiities (tasks) to run on a host.
- Task an action to be performed on a target endpoint.

A play is a YAML dictionary
A task is an ordered list in yaml

hosts: set at a play level in the playbook.  If hosts is a group in the inventory file, then the play is run on all members of the group

Tasks are ansible modules.  Example: command, script, yum, service

Run playbook: ansible-playbook playbookname.yml


Sample playbooks:
```
-
    name: 'Execute a date command on localhost'
    hosts: web_node1
    tasks:
        -
            name: 'Execute a date command'
            command: date
        -
            name: 'Execute a command to display hosts file'
            command: cat /etc/hosts
```

Sample playbook with to plays:
```
-
    name: 'Execute command to display date on web_node1'
    hosts: web_node1
    tasks:
        -
            name: 'Execute a date command'
            command: date
-
    name: Execute a command to display hosts file contents on web_node2
    hosts: web_node2
    tasks:
        -
            name: Execute a command to display hosts file
            command: cat /etc/hosts
```
Sample playbook with several plays and targets
```
-
    name: 'Stop the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Stop the web services on web server nodes'
            command: 'service httpd stop'
-
    name: 'Shutdown the databases on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Shutdown the databases on db server nodes'
            command: 'service mysql stop'
-
    name: 'Restart all servers'
    hosts: all_nodes
    tasks:
        -
            name: 'Restart all servers'
            command: '/sbin/shutdown -r'
-
    name: 'Start the databases on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Start the databases on db server nodes'
            command: 'service mysql start'
-
    name: 'Start the web servers on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Start the web servers on web server nodes'
            command: 'service httpd start'
```
## Ansible Modules
Modules include...
- System 
- Commands - execute commands or script
- Files
- Database
- Cloud
- Windows 

Command module - execute command on a remote endpoint

Script module - Ansible controller machine transfers sript to target and then runs

Service module - Manage Start, Stop, Restart
- state=started - we want the state of the service to be started. If not running start or if started skip

Example playbooks using other modules:
```
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
            name: 'Add web_user'
            user:
                name: web_user
                uid: 1040
                group: developers
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
 ```
## Ansible Variables
Use variables in playbook or separate file to loop playbooks through multiple devices

Example use of variables with a vars list included in the playbook:
```
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Print my car model'
            command: 'echo "My car''s model is {{ car_model }}"'
        -
            name: 'Print my country'
            command: 'echo "I live in the {{ country_name }}"'
        -
            name: 'Print my title'
            command: 'echo "I work as a {{ title }}"'
    vars:
        car_model: 'BMW M3'
        country_name: 'USA'
        title: 'Systems Engineer'
```

## Ansible Conditionals

## Ansible Loops
