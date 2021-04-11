# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant API/syntax version. Do not touch.
VAGRANTFILE_API_VERSION = "2"
# IMAGE_NAME = "bento/centos-7.8"
IMAGE_NAME = "centos/7"
MASTER_NODE_COUNT = 1
MANAGED_NODE_COUNT = 1
WORKER_NODE_COUNT = 1

NODE_IP_BASE = "172.16.0"
POD_NETWORK_CIDR = "192.168.0.0/16"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = IMAGE_NAME
	# for vagrant vb-guest plugin
	config.vbguest.installer_options = { allow_kernel_upgrade: true }
	# local machine path / VM mount path
	config.vm.synced_folder "./volume", "/vm_data"
	config.ssh.insert_key = false

	# Master node
	(1..MASTER_NODE_COUNT).each do |i|
		config.vm.define "k8s-master-#{i}" do |master|
			# Master node IP
			node_ip = "#{NODE_IP_BASE}.#{i+10}"
			node_name = "k8s-master-#{i}"
			node_ssh_port = "301#{i+10}"

			master.vm.hostname = node_name
			master.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 2
			end

			# master.vm.network "private_network", ip: node_ip
			# eth0: internal NAT, eth1 being public
			master.vm.network "public_network", ip: node_ip
			# # ssh port forwarding
			# master.vm.network "forwarded_port", guest: 22, hosts: node_ssh_port, auto_correct: false, id: "ssh"

			# master.vm.provision "shell",
			# 	inline: "ip route replace default via 172.30.1.254 dev eth1"

			
			

			# Ansible provisioning
			master.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/playbooks/init-k8s-master.yaml"
				ansible.galaxy_roles_path = "ansible/roles"
				# ansible.verbose = "vvv"

				ansible.extra_vars = {
					node_name: node_name,
					node_ip: node_ip,
					master_node_user: 'vagrant',
					master_node_group: 'vagrant',
					master_apiserver_advertise_address: node_ip,
					pod_network_cidr_block: "#{POD_NETWORK_CIDR}"
				}
			end
		end
	end

	# Managed node
	(1..MANAGED_NODE_COUNT).each do |i|
		config.vm.define "k8s-managed-#{i}" do |managed|
			node_ip = "#{NODE_IP_BASE}.#{i + 20}"
			node_name = "k8s-managed-#{i}"

			# managed.vm.network "private_network", ip: node_ip
			# eth0: internal NAT, eth1 being public
			managed.vm.network "public_network", ip: node_ip
			managed.vm.hostname = node_name

			managed.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 2
			end

			managed.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/playbooks/init-k8s-worker.yaml"
				ansible.galaxy_roles_path = "ansible/roles"
				ansible.extra_vars = {
					node_name: node_name,
					node_ip: node_ip
				}
			end
		end
	end

	# Woker node
	(1..WORKER_NODE_COUNT).each do |i|
		config.vm.define "k8s-worker-#{i}" do |node|
			node_ip = "#{NODE_IP_BASE}.#{i + 30}"
			node_name = "k8s-worker-#{i}"

			# eth0: internal NAT, eth1 being public
			# node.vm.network "private_network", ip: node_ip
			node.vm.network "public_network", ip: node_ip
			node.vm.hostname = node_name
			
			node.vm.provider "virtualbox" do |virtualbox|
				virtualbox.memory = 2048
				virtualbox.cpus = 2
			end

			node.vm.provision "ansible" do |ansible|
				ansible.playbook = "ansible/playbooks/init-k8s-worker.yaml"
				ansible.galaxy_roles_path = "ansible/roles"
				# ansible.verbose = "vvv"

				ansible.extra_vars = {
					node_name: node_name,
					node_ip: node_ip
				}
			end
		end
	end
end


