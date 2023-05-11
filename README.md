# Ansible
### Ansible Cheat Sheet


For this exercize we are going to create 3 VM, using vagrant and virtual box, run the commant **vagrant init** to create new vagrant file, and replace the content with the following one, and run **vagrant up**, after the VM are created we need to provide ssh connection, to all VM. Generate SHH-KEY, and copy to all VM from tertminal.


    ssh-copy-id - /path/to/ user@server/ip

    servers=[
    {
        :hostname => "ansible",
        :ip => "192.168.56.10",
        :box => "ubuntu/jammy64",
        :ram => 712,
        :cpu => 1
    },
        {
        :hostname => "web",
        :ip => "192.168.56.11",
        :box => "ubuntu/jammy64",
        :ram => 712,
        :cpu => 1
    },
    {
        :hostname => "db",
        :ip => "192.168.56.12",
        :box => "ubuntu/jammy64",
        :ram => 712,
        :cpu => 1
    }
    ]

    Vagrant.configure(2) do |config|
        servers.each do |machine|
            config.vm.define machine[:hostname] do |node|
                node.vm.box = machine[:box]
                node.vm.hostname = machine[:hostname]
                node.vm.network "private_network", ip: machine[:ip]
                node.vm.provider "virtualbox" do |vb|
                    vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
                end
            end
        end
    end


Create inventory file **inventory** with following content

192.168.56.11
192.168.56.12
192.168.56.12


Check the connection from the nost pc run, from the folder where is **inventroy** file

    ansible -u vagrant all --key-file ~/.ssh/vbox -i invertory -m ping

If you create ansible.cfg file in the working directory you can use the following command

    ansible all -m ping

ansible.cfg 

    [defaults]
    # tell which inventory file to use if not defined will use the defulat one located in /etc/ansible/host
    inventory = invertory
    # which ssh key to use for connection
    private_key_file = ~/.ssh/vbox
    # The default user to use when connecting to remote hosts. If not specified, Ansible will use the current user
    remote_user = vagrant

    ansible all -m ping

Update to all vm

    ansible all -m apt -a update_cache=true --become --ask-become-pass

install modules

    ansible all -m apt -a name=tree --become --ask-become-pass


## Create playbook for apache
Create new file **apache_install.yml** an add the following content

    ---

    - hosts: web
    become: true
    tasks:

    - name: install apache2 package
        apt:
        name: apache2
    - name: install php
        apt:
        name: libapache2-mod-php

Run the following commant to install the apache on one VM - web

    ansible-playbook --ask-become-pass apache_install.yml

## Example when using WHEN option

---
- hosts: web
  become: true
  tasks:

  - name: install apache2 and php on ubuntu os
    service:
      name: 
       - apache
       - libapache2-mod-php
      state: latest
      update_cache: yes
    when: ansible_distribution == "Ubuntu"










