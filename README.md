## Description
Installs latest version of Arvados on Ubuntu-based systems.

# Assumptions
* You have internal DNS records for:
 * arvados-api
 * arvados-keep
 * arvados-keep0
 * arvados-shell
 * arvados-workbench
 * arvados-postgres
 * arvados-crunch
* You have external DNS records for:
$CLUSTERID.$EXTERNALDOMAIN
workbench.$CLUSTERID.$EXTERNALDOMAIN
workbench2.$CLUSTERID.$EXTERNALDOMAIN
keep.$CLUSTERID.$EXTERNALDOMAIN
git.$CLUSTERID.$EXTERNALDOMAIN
webshell.$CLUSTERID.$EXTERNALDOMAIN
collections.$CLUSTERID.$EXTERNALDOMAIN
*.collections.$CLUSTERID.$EXTERNALDOMAIN
download.$CLUSTERID.$EXTERNALDOMAIN
ws.$CLUSTERID.$EXTERNALDOMAIN

# Installation Instructions
* Set up external DNS records
* Set up internal DNS records
* Stand up VMs/machines/containers to be used for internal hosts
 * Because of irritating things w/r/t containerization, you'll want `arvados-api, arvados-keep0, arvados-shell` and `arvados-crunch` to be VMs or bare metal rather than containers.
* Make sure you can ssh from your staging host (where you cloned this repo) to each machine as root without errors or complaints. This is most easily done with `ssh-copy-id`.
* Edit `inventory` file to include hosts used for roles if necessary (usually this will be handled by your creation of internal DNS records).
* Edit variables in `group_vars/all` file
* Run `arvados-playbook --user=root -i inventory arvados_setup.yml`
