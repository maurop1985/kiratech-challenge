# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "docker1" do |d1|
    d1.vm.box = "centos/8"
    d1.vm.hostname = "docker-1"
    d1.vm.network "private_network", ip: "192.168.33.10"
    d1.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yaml"
    end
  end
  
#  config.vm.define "docker2" do |d2|
#    d2.vm.box = "centos/8"
#    d2.vm.hostname = "docker-2"
#    d2.vm.network "private_network", ip: "192.168.33.20"
#  end
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
