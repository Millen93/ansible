# Ansible script for Securely and Safely Upgrading Your System

## Define Your Main Service Which Should Not be Updated on Target Hosts
To do this, configure the file:
```shell
sudo su root
#Change CHANGEME to your service name, for example qr(^ssh). This will disable autoreload of PostgreSQL
echo -e '$nrconf{override_rd} = { \n   qr(^CHANGEME) => 0, \n};' >> /etc/needrestart/conf.d/main_services.conf
```

## Configure the Environment

Install ansible
```shell
sudo apt update && sudo apt install -y python3-pip sshpass git
pip3 install ansible
```

Edit the inventory file:
```shell
nano inventory
```

## Run the Upgrade Ansible Playbook
This script will update and upgrade your system, and return the values of services that need to be restarted, as well as hosts that need to be rebooted:
```shell
ansible-playbook ./playbooks/upgrade_system.yml
```
You can view the output in the **./logs** directory. Also, pay attention to the **NEED_RESTART** file, which contains hosts that you should reboot to update the version of the kernel.

## Run Restart Ansible playbook
If you want to restart services stored in **./logs/<ip_addr>**, run:
```shell
ansible-playbook ./playbooks/restart_services.yml
```
