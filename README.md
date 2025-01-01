# ansible-playbooks

This repository contains Ansible playbooks and configuration files used to automate the deployment of a static web application on AWS EC2 instances. The playbooks automate the setup of Apache HTTP server (`apache2`), server configuration, and the deployment of static web content. The objective is to simplify and automate the process of hosting static websites on EC2 instances using Ansible.


#### AWS setup
Spin up 3(can be any number of instance) EC2 instances and add inbound rule HTTP at port 80 from anywhere. Setup passwordless authentication using ssh-keys or password.
By default password is disabled on EC2 instance. 

###### Using Password
Go to the file 
```
/etc/ssh/sshd_config.d/60-cloudimg-settings.conf
```

Update 
```
PasswordAuthentication yes
```


Restart SSH -> `sudo systemctl restart ssh`

###### Using Public Key
```
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
```

- ssh-copy-id: This is the command used to copy your public key to a remote machine.
- -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
- "-o IdentityFile ": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
- ubuntu@: This is the username (ubuntu) and the IP address of the remote server you want to access.

#### Ansible Setup 
Install and Configure Ansible on the Host machine.
```
sudo apt update
sudo apt install python3  # Install Python3
python --version  # or python3 --version
sudo apt install ansible -y  # Install ansible
ansible --version
```

#### Ansible Inventory file
After setting up passwordless authentication, add the details of the managed nodes (EC2 instances) in the inventory file inventory.ini
```
ubuntu@<public_ip_address>
ubuntu@<public_ip_address>
ubuntu@<public_ip_address>
```

### Ansible playbook
Create an Ansible playbook (ec2-deploy.yaml) with the following tasks:

- Install the Apache web server (apache2) to host the web application.
- Copy the files from the host machine to the EC2 instances.

Run the command

```
ansible-playbook -i inventory.ini ec2-deploy.yaml
```

This command will connect to the managed nodes (EC2 instances), install Apache (apache2), and deploy the files.

To access the web app, copy any of the EC2 instance's public IP addresses and paste it into your browser. The website will be displayed!



![clideo_editor_788ad8c17b284f66b9eea18393401708](https://github.com/user-attachments/assets/68724e76-d9b1-45e5-a723-9b15b4963858)




