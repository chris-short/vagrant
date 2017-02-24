# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Require YAML module
require 'yaml'

# Read YAML file with box details
servers = YAML.load_file('servers.yaml')

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

servers.each do |servers|
	config.vm.define servers["name"] do |srv|
		srv.vm.box = servers["box"]
		srv.vm.network "private_network", ip: servers["ip"]
		srv.vm.network :forwarded_port, guest: 22, host: servers["ssh_port"], id: "ssh", auto_correct: true
		srv.vm.hostname = servers["name"]
		srv.vm.synced_folder ".", "/vagrant", disabled: true
			config.vm.provider "virtualbox" do |vb|
        vb.gui = true
				#vb.customize ["modifyvm", :id, "--cpuexecutioncap", "40"]
				vb.name = servers["name"]
				vb.memory = servers["ram"]
				vb.cpus = servers["cpus"]
			end
		end
	end
	config.vm.provision :ansible do |ansible|
		ansible.playbook = "common.yml"
	end
end
