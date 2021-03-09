# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'ffi'

# Vagrant API/syntax version. Do not touch.
VAGRANTFILE_API_VERSION = "2"
# centos 7.6 대상으로 변경해야 한다.
IMAGE_NAME = "bento/ubuntu-18.04"
# centos
# IMAGE_NAME = "bento/centos-7.6"
NODE_COUNT = 2

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = IMAGE_NAME
	config.ssh.insert_key = false

	if FFI::Platform::IS_MAC
		# do something for mac
	end

	config.vm.provider "virtualbox" do |virtualbox|
		virtualbox.memory = 2048
		virtualbox.cpus = 2
	end

	# Master node
	config.vm.define "k8s-master" do |master|
		#master.vm.network "private_network", ip: "192.168.50.10"
		master.vm.network "private_network", ip: "172.16.0.10"
		master.vm.hostname = "k8s-master"
		# Ansible provisioning
		master.vm.provision "ansible" do |ansible|
			# API, etcd, scheduler, controller manager, core-dns, dashboard
			ansible.playbook = "ansible/k8s-master-playbook.yml"
			ansible.extra_vars = {
				ansible_python_interpreter: "/usr/bin/python3",
				node_name: "k8s-master",
				#node_ip: "192.168.50.10", # IP 대역이 겹치면 네트워크 문제가 있으니 대역 잘 살펴보자.
				node_ip: "172.16.0.10", 
				pod_network_cidr_block: "192.168.0.0/16"
			}
		end
	end

	# Woker node
	(1..NODE_COUNT).each do |i|
		config.vm.define "k8s-node-#{i}" do |node|
			node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
			node.vm.hostname = "node-#{i}"
			node.vm.provision "ansible" do |ansible|
				# istio(설치 순위가 낮다)
				ansible.playbook = "ansible/k8s-node-playbook.yml"
				ansible.extra_vars = {
					ansible_python_interpreter: "/usr/bin/python3",
					node_name: "node-#{i}",
					node_ip: "192.168.50.#{i + 10}"
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


