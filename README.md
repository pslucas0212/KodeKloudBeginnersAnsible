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
Remove the comment from this line - host_key_checking = False
> This not recommended for production

3. From the projects directory where you created the inventory.txt file run the following ansible command
```
ansible target1 -m ping -i inventory.txt
ansible target2 -m ping -i inventory.txt
```
> You should receive a successful ping
