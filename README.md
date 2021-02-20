# Kiratech Challenge
[![Build Status](https://travis-ci.com/maurop1985/kiratech-challenge.svg?branch=main)](https://travis-ci.com/maurop1985/kiratech-challenge)  

An Ansible Playbook to provision 2 VM Centos with the following features:
- Assure the availability of at least 40GB about Docker partition
- Docker setup on 2 VM
- Docker configuration:  
  1. Expose Docker Daemon REST API in secure mode
  2. Docker daemon starts at boot
- Docker Swarm configuration:
  1. Securely access
  2. Up&Running to deploy services from local host

All push actions, on master branch, trigger a Travis CI pipeline to check playbook syntax errors.

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
**docker_swarm_manager** group must contain one and only one VM.   
**docker_swarm_worker** group must contain the other VM.

IP addresses specified in the inventory under _ansible_ssh_host_ will be used to configure docker swarm cluster.

## Variables
Variables are in [provisioning/variables.yaml](provisioning/variables.yaml)
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
Run playbook with the below command:  
`ansible-playbook -i provisioning/inventory provisioning/playbook.yaml`

## Considerations about roles
Playbook runs 5 roles to achieve the goal:
1. vm_configuration (configure virtual machines before docker installation)
2. ansible-role-docker (owner of docker installation tasks)
3. docker_secure
4. docker_swarm (initialize 2 nodes docker swarm with 1 manager and 1 worker)
5. docker_test (test service deploy from local host)


Playbook includes 2 community roles.  
[ansible-role-docker](https://github.com/geerlingguy/ansible-role-docker) - it is used to install docker and it has been chosen for these reasons:
- reliable creator
- role ready to use without further configuration or tricks
- role documented very well
- it configures docker daemon to start at boot

[role-secure-docker-deamon](https://github.com/alexinthesky/role-secure-docker-daemon) - it is used in the custom role docker_secure and it tasked of create:
- CA Key and Certificate
- Server Key and Certificate
- Client Key and Certificate