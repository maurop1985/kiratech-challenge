# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.insert_key = false

  config.vm.define "docker1" do |d1|
    d1.vm.box = "centos/8"
    d1.vm.hostname = "docker1"
    d1.vm.network "private_network", ip: "192.168.33.10"
  end
  
  config.vm.define "docker2" do |d2|
    d2.vm.box = "centos/8"
    d2.vm.hostname = "docker2"
    d2.vm.network "private_network", ip: "192.168.33.20"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "provisioning/inventory"
    ansible.playbook = "provisioning/playbook.yaml"
    ansible.groups = {
      "docker_swarm_manager" => ["docker1"],
      "docker_swarm_worker" => ["docker2"],
    }
  end

end
