## Step 1:- 

create two ec2, one is on linux and another one is on ubuntu with two different key pair

## Step 2:-

   save the key pair on the local system and change the permisson

         chmod 600 <keyname>

## Step 3:-

  create a inventory.yaml/inventory.ini file and it have host-name keypair path and name of the user
   
    
inventory.yaml
       
              all:
                  hosts:
                    ubuntu_server:
                      ansible_host: 54.174.97.10
                      ansible_user: ubuntu
                      ansible_ssh_private_key_file: /home/akshay-r/ansible/uk.pem
                      ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
                    linux_server:
                      ansible_host: 34.229.96.140
                      ansible_user: ec2-user
                      ansible_ssh_private_key_file: /home/akshay-r/ansible/lk.pem
                      ansible_ssh_common_args: '-o StrictHostKeyChecking=no'


## step 4:-

   install and update ansible
   
        sudo apt update
        sudo apt install ansible -y
        ansible --version

## Step 5:-

  to cheack the 2 servers connected to ansible
  
      ansible all -i hosts.ini -m ping

## Step 6:-

   install nginx on ubundu
   
      ansible ubuntu_server -i inventory.yaml -m apt -a "name=nginx state=present update_cache=yes" -b
   
   start and enable nginx on ubuntu
   
      ansible ubuntu_server -i inventory.yaml -m service -a "name=nginx state=started enabled=yes" -b
   
   cheack service status
   
      ansible ubuntu_server -i inventory.yaml -m service -a "name=nginx state=started"

   install nginx on linux
   
      ansible linux_server -i inventory.yaml -m yum -a "name=nginx state=present" -b
   
   start and enable nginx on linux
   
      ansible linux_server -i inventory.yaml -m service -a "name=nginx state=started enabled=yes" -b
  
    













