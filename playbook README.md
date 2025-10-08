## step 1:-
    create one ec2 server

step 2:- install ansible
```
     sudo apt update
     sudo apt install -y ansible
     ansible --version
```
step 3:-
     create a folder and save key pair

step 4:-
     create inventory.yaml
     
        all:
          vars:
            ansible_become: true
          children:
            frontend:
              hosts:
                worker1:
                  ansible_host: 3.88.200.186
                  ansible_user: ubuntu
                  ansible_ssh_private_key_file: /home/akshay-r/ansible-playbook/uk.pem
                  ansible_ssh_common_args: '-o StrictHostKeyChecking=no'


step 5:- 
    create an playbook.yaml (it contain the jobs to do in the server)
    
          - name: Install and start Nginx on worker node
            hosts: frontend
            become: yes
            tasks:
             - name: Update apt package index
               apt:
                 update_cache: yes
             - name: Install nginx
               apt:
                 name: nginx
                 state: present
             - name: Enable and start nginx service
               service:
                 name: nginx
                 state: started
                 enabled: yes

step 6:-
    run the command to ansible
    
  ansible-playbook `playbook.yaml`
      
            
ansible-playbook -i `inventory.yaml` `playbook.yaml`






