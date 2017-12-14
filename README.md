# Ansible rolling upgrade demonstration
Prerequisites:
- Ubuntu 14.04

Steps to perform:
- clone this repo to ansible node
- insert your ip adresses in `production` inventory
- perform `ansible-playbook -i production installation.yml`
- perform `ansible-playbook -i prdocution rolling.yml` for rolling upgrades
Notice: you can change global variables in `group_vars/production` to play around with different parameters
