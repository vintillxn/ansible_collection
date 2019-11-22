# nagios_core

This playbook was created after my needs in order to install nagioscore and nagiosplugins from scratch, for the moment only for ubuntu.
The installations are based on officially sources, nothing nasty here.
The playbook has 4 roles:
1/ install_dependencies for ansible and nagios
2/ install_nagios_core
3/ install_nagios_plugins_ubuntu
4/ config_nagios


In order to use the last role, config_nagios, you need to create the ansible inventory file as below:

```
[Nagios_Core]
#here you'll declare the nagiosserver for the installation
nagios-vm ansible_host=10.10.10.10  ansible_ssh_user=nagios ansible_ssh_pass=nagios

[Group1]
myvm1 ansible_connection=ssh ansible_ssh_host=10.10.10.11 sup_ip_addr=10.10.10.11 ansible_ssh_user=user ansible_ssh_pass=password

[Group2]
myvm2 ansible_connection=ssh ansible_ssh_host=10.10.10.12 sup_ip_addr=10.10.10.12 ansible_ssh_user=user ansible_ssh_pass=password
```

In the playbook itself, there is a var which you can set it wherever you'd like to install nagios (for example /opt/nagios/)
For the first 3 roles, the name is pretty obvious.
The tricky part is for the last role, config_nagios. This one was created to:
  - copy the nagios config file in /opt/nagios/etc/nagios.cfg path and adapt in it the paths used for nagios
  - in the nagios config file I've decided to use a config directory called Servers where I'll store the config file for all the hosts which will be monitored
  - create the nagios host config file. This file is created by checking the ansible inventory file and it takes every host except Nagios_Core, and group and add it into Nagios monitoring
  - for each group in ansible inventory file, is creating as well a group in nagios
  - restarts the nagios service in order for the changes to be applied.

