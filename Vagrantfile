# -*- mode: ruby -*-
# vi: set ft=ruby :

$count_nodes = 3

####### Common parameters

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update=false
  config.vm.provider "virtualbox" do |vb|
     vb.gui = false
     vb.memory = "1024"
     vb.cpus=2
	end

####### define nodes 

(1..$count_nodes).each do |i|
    config.vm.define "node#{i}" do |node|
        node.vm.network "public_network", ip: "10.12.112.#{34+i}"
        node.vm.hostname = "node#{i}"
		end
	end

####### apply ansible playbook to nodes

  config.vm.provision "ansible_local" do |ansible|
     ansible.playbook = "setup.yml"
	 ansible.compatibility_mode = "2.0"
	 ansible.inventory_path = "staging"
	end
end