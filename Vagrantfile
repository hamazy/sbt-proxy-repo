# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "chef/centos-6.6"
  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--ostype", "RedHat_64"]
    v.memory = 1024
  end
  config.vm.network "forwarded_port", guest: 8081, host: 8081
end
