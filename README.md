# kubernetes-on-vagrant-using-ansible

## Prerequisites
- Vagrant
- VirtualBox
- Ansible

## Kubernetes Cluster
|VM name| IP address | CPUs | Memory |
|---|---|---|---|
|k8s-master| 172.16.0.10 | 2 | 2 GB |
|k8s-node-1| 172.16.0.11 | 1 | 2 GB |
|k8s-node-2| 172.16.0.12 | 1 | 2 GB |


## Installing Kubernetes on Vagrant


## vagrant command sheet

`vagrant up` - starting a VM
`vagrant halt` - stoping a VM
`vagrant destroy` - cleaning up a VM
`vagrant box list` - listing boxes
`vagrant ssh <VM-name>` - connecting a VM
`vagrant provision` - provisioning VMs with Anisble