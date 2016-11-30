# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.2"
  config.vm.box_check_update = true
  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum update -y
  SHELL

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "tests/provision.yml"
  end
end
