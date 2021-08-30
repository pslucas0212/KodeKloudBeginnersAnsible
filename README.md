# Kode Kloud Ansible for the Absolute Beginners Course Notes
My notes from the Kode Kloud Ansible for the Absolute Beginners Course

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
