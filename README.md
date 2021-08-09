## Description
Installs the latest version of Arvados on seven Ubuntu-based hosts.

# Introduction
[Arvados](https://www.arvados.org/) is a federated data management and CWL workflow processing platform developed by [Curii](https://www.curii.com/). It was originally developed to support biomedical scientific environments, but its scope has since expanded to include data and workflow execution more generally, leading it to be deployed in a wide range of applications.

At BioTeam, we find Arvados interesting because it helps fill several important needs we've found that crop up again and again at our clients:
* Tracking metadata and data provenance
* Ensuring you never "lose track" of data thanks to content addressing
* Relative ease of presentation of results from workflows
* Built-in tiering and archiving
* Handling user/group permissions and ownership between different authentication schemes (LDAP, OIDC, etc.) consistently, assisting in data sharing
* Federation and cross-instance workflow execution between labs or between organizations

# Assumptions
* You are using Ubuntu 18.04. This playbook should work with Ubuntu 20.04, but you may encounter some strange issues with things related to Python or other application environments. (For example, you may have to change the Python version in roles/config/main.yml from 3.6 to 3.8. Similar issues may crop up with Ruby.)
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
* You have a reverse-proxy server on an eighth host terminating TLS for you and distributing web requests behind it to hosts responsible for running Arvados services.

# Installation Instructions
* Set up external DNS records.
* Set up internal DNS records.
* Set up a reverse-proxy server to terminate TLS and distribute external web requests to the appropriate hosts. We like to use Caddy for this purpose. An example Caddyfile is included in the root of this repository.
* Stand up VMs/machines/containers to be used for internal hosts.
  * Because of irritating things w/r/t containerization, you'll want `arvados-api, arvados-keep0, arvados-shell` and `arvados-crunch` to be VMs or bare metal rather than containers (i.e. system containers like `lxc`).
* Make sure you can ssh from your staging host (where you cloned this repo) to each machine as root without errors or complaints. This is most easily done with `ssh-copy-id` on machines you haven't prepared with `cloud-init` or similar.
* Edit `inventory` file to include hosts used for roles if necessary (usually this will be handled by your creation of internal DNS records).
* Edit variables in `group_vars/all` file.
* Run `arvados-playbook --user=root -i inventory arvados_setup.yml`.
