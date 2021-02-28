# ansible and vagrant
## set up vagrant
### install vagrant 
see https://www.vagrantup.com/docs/installation
- download vagrant for host os
- copy vagrant to /usr/bin/vagrant
- vagrant -v :: Vagrant 2.2.14
### install/updata virtualbox
see https://www.virtualbox.org/wiki/Linux_Downloads
- download and install deb file
- start virtualbox, about version=Version 6.1.18 r142142 (Qt5.12.8)
- repair plugins :: `vagrant plugin expunge --reinstall`
- add box :: `vagrant box add geerlingguy/centos7`
  box: Successfully added box 'geerlingguy/centos7' (v1.2.24) for 'virtualbox'!
- create a virtual server :: `vagrant init geerlingguy/centos7`
- start server :: `vagrant up`
  * error msg
```
 There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["startvm", "08e902b0-e939-4734-9c1f-dbe3fb3af405", "--type", "headless"]

Stderr: VBoxManage: error: The virtual machine '02-vagrant-virtualbox_default_1614541739873_55999' has terminated unexpectedly during startup with exit code 1 (0x1)
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component MachineWrap, interface IMachine
```
  * fix: delete all instances of virtalbox and reinstall
  
- log on to server :: `vagrant ssh`

- add playbook.yml file to current directory
``` yaml
---
- hosts: all
  become: yes

  tasks:
  - name: ensure chrony (time sync) is installed
    yum:
      name: chrony
      state: present

  - name: ensure chrony is running
    service:
      name: chronyd
      state: started
      enabled: yes

      
```
- update the Vagrantfile with the following lines: 
(after config.vm.box = "geerlingguy/centos7")
```
  # provision configuration for ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
```
- execute : `vagrant provision`
this will execute the playbook

- cleanup :: vagrant destroy



  

