# Ansible rolling upgrade demonstration with usage of rollback mechanics and ansible-vault
## Overview:
Stack overview:
- Ansible configures stack with 4 nodes. One node is nginx as front-end load balancer for 3 nodes with test application https://bitbucket.org/ZaRDaK/devops-rolling-release/overview

Tested on Amazon Web Services

## Demo installation:
Prerequisites:
- 1 ansible instance with preinstalled ansible and git
- 1 raw node for nginx role (Ubuntu 14.04)
- 3 raw nodes for application role (Ubuntu 14.04)
- ansible node should have ssh access to other nodes as `ubuntu` user with sudo no password permission (you can change ansible user in `production` file)
- nginx node should have http (port 80) access to application nodes (you can change default port in `group_vars/production` file)
- user should have http (port 80) access to nginx node for viewing the result

Steps to perform initialization of stack:
- clone this repo to ansible node
- create `.vault_pass` file and insert there passphrase which you can find at the end of this README.md
- insert your ip adresses in `production` inventory
- perform `ansible-playbook -i production initialization.yml --vault-password-file=.vault_pass` for init configuration of stack

Performing rolling upgrades demonstration:
- perform `ansible-playbook -i prdocution rolling.yml --vault-password-file=.vault_pass` for rolling upgrades

Performing rollback demonstration:
- change `app_version` variable in `group_vars/production` to corrupted version `'v1.1.0'`
- perform `ansible-playbook -i prdocution rolling.yml --vault-password-file=.vault_pass` for rolling upgrades

Notice: you can change global variables in `group_vars/production` to play around with different parameters

## Features:
List of features:
- you can initialize with failing application. Ansible code will warn you and stop failing app
- you can initialize already initialized hosts. Ansible will perform no actions on already configured hosts
- rollback mechanics implemented into `rolling.yml` playbook, so if application fail during run it will rollback to previous version
- `group_vars/production` file is encrypted using ansible-vault mechanics
- there are a lot of tags implemented. For example you could run `ansible-playbook -i production initialization.yml --vault-password-file=.vault_pass --tags "nginx"` for updating nginx load balancer only
- you can also perform initialization with rolling upgrades as well as rolling upgrades on full stack by running `ansible-playbook -i production initialization.yml --vault-password-file=.vault_pass --tags "nginx"` and `ansible-playbook -i prdocution rolling.yml --vault-password-file=.vault_pass`

## Bugs:
List of bugs:
- there is a known bug with nginx repo keys within ansible. If you get an error while running nginx role just rerun the same playbook

Vault passphrase `ansible_task`
