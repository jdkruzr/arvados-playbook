## Description
Installs Arvados 2.2 on Ubuntu-based systems. Includes an Active Directory domain joining playbook for each host for the sake of management.

# Assumptions
* You have DNS entries for:
 * arvados-api
 * arvados-keep
 * arvados-keep0
 * arvados-shell
 * arvados-workbench
 * arvados-workbench2
 * arvados-ws
 * arvados-postgres

# Installation Instructions
* Edit `inventory` file to include hosts used for roles
* Edit variables in `group_vars/all` file:
  * 
* Run `arvados-playbook linux_domain_join.yml 
