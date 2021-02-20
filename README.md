# Kiratech Challenge
[![Build Status](https://travis-ci.com/maurop1985/kiratech-challenge.svg?branch=main)](https://travis-ci.com/maurop1985/kiratech-challenge)  

An Ansible Playbook to provision 2 VM Centos with the following features:
- Assure the availability of at least 40GB about Docker partition
- Docker setup on 2 VM
- Docker configuration:  
  1. Expose Docker Daemon REST API in secure mode
  2. Docker daemon starts on vm boot
- Docker Swarm configuration:
  1. Securely access
  2. Up&Running to deploy services from local host

All push action on master branch trigger a Travis CI pipeline to check playbook syntax errors.

**Optional features are not covered from this playbook**

## Requirements
Install playbook dependencies (on the local host):  
`ansible-galaxy install -r requirements.yaml`  
`ansible-galaxy collection install community.docker`

Install python packages (on the local host):  
`pip install docker`  
`pip install docker-compose`  
`pip install PyYAML`

## Inventory File

Replace properly in the [inventory](provisioning/inventory):
- hostname and ip of 2 virtual machine
- ansible_user
- private key path 

This is the inventory file of my dev environment used in combination with Vagrant
```
docker1 ansible_ssh_host=192.168.33.10 ansible_ssh_port=22 ansible_user='vagrant' ansible_ssh_common_args='-o StrictHostKeyChecking=no' ansible_ssh_private_key_file='path_of_insecure_private_key'
docker2 ansible_ssh_host=192.168.33.20 ansible_ssh_port=22 ansible_user='vagrant' ansible_ssh_common_args='-o StrictHostKeyChecking=no' ansible_ssh_private_key_file='path_of_insecure_private_key'

[docker_swarm_manager]
docker1

[docker_swarm_worker]
docker2
```
It's required that one of two VM is part of [docker_swarm_manager] group, while the other VM is in the [docker_swarm_worker] group

IP addresses specified in the inventory under _ansible_ssh_host_  will be used to configure docker swarm cluster,

## Variables

```sh
# VM-CONFIGURAION VARIABLES
docker_partition_min_size_gb:           #default=40
#Device to configure docker storage partition
docker_data_disk:                       #default=/dev/sdb
#Mountpoint
docker_data_path:                       #default=/var/lib/docker  
#Volume group name
volume_group_name:                      #default=docker_vg
#Logical volume name
logical_volume_name:                    #default=docker_lv 


# DOCKER SECURITIZATION
#CA Variable
dds_country:                    #default=IT
dds_state:                      #default=Rome
dds_locality:                   #default=Rome
#Path where server certificates will be created
dds_server_cert_path:           #default=/etc/docker
# Path where client certificates will be created
dds_client_cert_path:           #default=/etc/docker/client
```

## Run
`ansible-playbook -i provisioning/inventory provisioning/playbook.yaml`

## Consideration about roles
Playbook runs 4 roles to achieve the goal:
1. vm_configuration
2. ansible-role-docker (owner of docker installation tasks)
3. docker_secure
4. docker_swarm
5. docker_test (test service deploy from local host)


It includes 2 community roles.  
[ansible-role-docker](https://github.com/geerlingguy/ansible-role-docker) - it is used to install docker ans has chosen for these reasons:
- creator reliability
- ready to use without further configuration or tricks
- very well documented

[role-secure-docker-deamon](https://github.com/alexinthesky/role-secure-docker-daemon) - it is used in the custom role docker_secure and it tasked of create:
- CA Key and Certificate
- Server Key and Certificate
- Client Key and Certificate

