# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/ubuntu2004"
  config.vm.hostname = "yolo.test"
  config.vm.network :private_network, ip: "192.168.56.0"
  config.vm.network "forwarded_port", guest: 3000, host: 3000, protocol: "tcp"
  config.vm.network "forwarded_port", guest: 5000, host: 5000, protocol: "tcp"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider :virtualbox do |v|
    v.memory = 2000
  end

  # Ansible provisioner.
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yaml"
  end
end
