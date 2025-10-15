## Step 1:-

Create one ec2(or any server) machine

## Step 2:-

Install ansible


    sudo apt update
    sudo apt install ansible -y
    ansible --version


## Step 3:-

Create one directory 

     mkdir nginx-role

## Step 4:-

  Save key file and change mod

     chmod 600 <keypair>

## Step 5:-

 Create inventory.yaml

    all:
      vars:
        ansible_become: true
      children:frontend:
        hosts:
          ubuntu_server:
            ansible_host: 3.110.166.132
            ansible_user: ubuntu
            ansible_ssh_private_key_file: /home/ubuntu/ansible/octkey.pem


## Step 6:-

   Test ssh connectivity

      ansible all -i <inventory.yaml> -m ping


## Step 7:-

 Create a role for the project

    ansible-galaxy init roles/nginx

 After this command a role created and under in the role folder nginx folder is created 
 open the nginx role and then somany folders are present like tasks, handlers etc

 ## Step 8:-

   Open roles/nginx/defaults/main.yml and edit the varible

     app_name: nginx
     app_user: "{{ ansible_user | default('ubuntu') }}"

## Step 7:-

 Open roles/nginx/tasks/main.yml and add taskes we want to perform

    # Nginx installation tasks
    - name: Update package cache on Debian/Ubuntu
      ansible.builtin.apt:
        update_cache: yes
      when: ansible_os_family == "Debian"
    - name: Install Nginx on Debian/Ubuntu
      ansible.builtin.apt:
        name: "{{ app_name }}"
        state: present
      when: ansible_os_family == "Debian"
      notify: restart nginx
    - name: Ensure Nginx is enabled and running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes

## Step 8:-

Open roles/nginx/handlers/main.yml and edit handler job

     - name: restart nginx
       ansible.builtin.service:
         name: nginx
         state: restarted

## Step 9:-

Create a playbook.yaml

    - name: Install Nginx on Ubuntu Server
      hosts: frontend
      become: yes
      roles:
        - nginx

## Step 10:-

Run the playbook

     ansible-playbook -i inventory.yaml playbook.yaml
