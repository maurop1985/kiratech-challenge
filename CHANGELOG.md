
# Change Log
All notable changes to this project will be documented in this file.

## 1.0.1
- Fix .ansible-lint in order to exclude imported roles
## 1.0.0
- Fix ansible-lint warnings
## 0.4.5
- Edit .travis.yml
## 0.4.4
- Edit .travis.yml
## 0.4.3
- Edit .travis.yml
## 0.4.2
- Add ansible-lint check to .travis.yml
## 0.4.1
- Fix travis filename
## 0.4.0
- Add travis.yaml for Travis CI integration
## 0.3.1
- Fix docker certificate
## 0.3.0
- Add "vm-configure" role to configure VM with software and storage prerequisites
- Add "docker-swarm" role to configure docker swarm cluster
- Delete docker-installation role and promote geerlingguy.docker role to playbook level
- Delete not utilized directory from roles path
- Add requirements.txt file
- Change directory structure
## 0.2.0
- Add docker-installation role
- Add docker-securitization role to create CA, Server and Client Certificate and enable TLS for docker dameon connection 
## 0.1.0
Init project
- Provisioning of 2 virtual machine with vagrant

