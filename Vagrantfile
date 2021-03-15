# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'ffi'

# Vagrant API/syntax version. Do not touch.
VAGRANTFILE_API_VERSION = "2"
IMAGE_NAME = "bento/centos-7.6"
NODE_COUNT = 2

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = IMAGE_NAME
	config.ssh.insert_key = false

	if FFI::Platform::IS_MAC
		# do something for mac
	end

	# Master node
	config.vm.define "k8s-master" do |master|
		master.vm.network "private_network", ip: "172.16.0.10" #,auto_config: true
		master.vm.hostname = "k8s-master"
		
		master.vm.provider "virtualbox" do |virtualbox|
			virtualbox.memory = 2048
			virtualbox.cpus = 2
		end

		# Ansible provisioning
		master.vm.provision "ansible" do |ansible|
			ansible.playbook = "ansible/k8s-master-playbook.yml"
			ansible.verbose = "vvv"
			ansible.extra_vars = {
				# ansible_python_interpreter: "/usr/bin/python3",
				node_name: "k8s-master",
				node_ip: "172.16.0.10", 
				pod_network_cidr_block: "192.168.0.0/16"
			}
		end
	end

	# Woker node
	(1..NODE_COUNT).each do |i|
		config.vm.define "k8s-node-#{i}" do |node|
			node.vm.network "private_network", ip: "172.16.0.#{i + 10}"
			node.vm.hostname = "k8s-node-#{i}"
			
			node.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 1
			end

			node.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/k8s-node-playbook.yml"
				ansible.verbose = "vvv"
				ansible.extra_vars = {
					# ansible_python_interpreter: "/usr/bin/python3",
					node_name: "k8s-node-#{i}",
					node_ip: "172.16.0.#{i + 10}"
				}
			end
		end
	end

	# Import local setting
	# local_setting = 'Vagrantfile.local'
	# external = File.read local_setting 
	# if File.exist?(local_setting)
	# 	eval external 
	# if not external.nil?
	# 	end

end


