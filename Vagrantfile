# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'ffi'

# Vagrant API/syntax version. Do not touch.
VAGRANTFILE_API_VERSION = "2"
IMAGE_NAME = "bento/centos-7.8"
MASTER_NODE_COUNT = 1
MANAGED_NODE_COUNT = 1
WORKER_NODE_COUNT = 1
IP_BASE = "192.168.50"
POD_NETWORK_CIDR = "192.168.0.0/16"


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = IMAGE_NAME
	# local machine path / VM mount path
	config.vm.synced_folder "./volume", "/vm_data"
	config.ssh.insert_key = false

	if FFI::Platform::IS_MAC
		# do something for mac
	end

	# Master node
	(1..MASTER_NODE_COUNT).each do |i|
		config.vm.define "k8s-master-#{i}" do |master|
			master.vm.network "private_network", ip: "#{IP_BASE}.#{i+10}" #,auto_config: true
			master.vm.hostname = "k8s-master-#{i}"
			
			master.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 2
			end

			# Ansible provisioning
			master.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/playbooks/init-k8s-master.yaml"
				ansible.galaxy_roles_path = "ansible/roles"
				# ansible.verbose = "vvv"

				ansible.extra_vars = {
					node_name: "k8s-master-#{i}",
					node_ip: "#{IP_BASE}.#{i+10}",
					master_node_user: 'vagrant',
					master_node_group: 'vagrant',
					# master_apiserver_advertise_address: "#{IP_BASE}.#{i+10}"
					master_apiserver_advertise_address: "#{IP_BASE}.#{i+10}",
					pod_network_cidr_block: "#{POD_NETWORK_CIDR}"
				}
			end
		end
	end

	# Managed node
	(1..MANAGED_NODE_COUNT).each do |i|
		config.vm.define "k8s-managed-#{i}" do |managed|
			managed.vm.network "private_network", ip: "#{IP_BASE}.#{i + 20}"
			managed.vm.hostname = "k8s-managed-#{i}"

			managed.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 2
			end

			managed.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/playbooks/init-k8s-worker.yaml"
				ansible.galaxy_roles_path = "ansible/roles"
				ansible.extra_vars = {
					node_name: "k8s-managed-#{i}",
					node_ip: "#{IP_BASE}.#{i + 20}",
				}
			end
		end
	end

	# Woker node
	(1..WORKER_NODE_COUNT).each do |i|
		config.vm.define "k8s-worker-#{i}" do |node|
			node.vm.network "private_network", ip: "#{IP_BASE}.#{i + 30}"
			node.vm.hostname = "k8s-worker-#{i}"
			
			node.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 2
			end

			node.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/playbooks/init-k8s-worker.yaml"
				ansible.galaxy_roles_path = "ansible/roles"
				# ansible.verbose = "vvv"

				ansible.extra_vars = {
					node_name: "k8s-worker-#{i}",
					node_ip: "#{IP_BASE}.#{i + 30}"
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


