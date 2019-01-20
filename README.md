# RedhatAnsible

An Red Hat Enterprise 3.7 setup with Ansible 2.3 VM intended for the Red Hat Certified Specialist in Ansible Automation exam (EX407)

The following VMs will be created

ansible1 - Ansible control host.
web1 - nginx web server.
web2 - nginx web server.
db1 - database server.
db2 - database server.
tower1 - Ansible Tower / AWX.

The Plan
===========

This is intend to be a run-through of a bunch of Ansible stuff to prepare for the EX407 exam.

You can replace roles and vm with your own if desired.

Task on control hosts (ansible)
================================

1. Create inventory.
2. ansible.cfg modification.
3. Install roles.
4. Create playbooks.
5. Run playbook to setup system.
6. Additional tasks.

```
vagrant ssh ansible1;
sudo su -
# Add to hosts file
TAB="$(printf '\t')"
cat << EOF >> /etc/hosts
192.168.44.102${TAB}${TAB}web1
192.168.44.103${TAB}${TAB}web2
192.168.44.104${TAB}${TAB}db1
192.168.44.105${TAB}${TAB}db2
192.168.44.106${TAB}${TAB}tower1
EOF
exit

# Verify
ping tower1

# ssh-copy-id

ssh-keygen -t rsa
for H in web1 web2 db1 db2 tower1; do
	ssh-copy-id vagrant@"$H";
done;

TAB="$(printf '\t')"
cat << EOF >> /home/vagrant/inventory
[web]
web1
web2

[db]
db1
db2

[tower]
tower1
EOF

# test ansible connectivity
ansible all -i inventory -m ping

ansible all -i inventory -m command -a "whoami" -b

# Create a config file for ansible
sudo su -
mkdir /etc/ansible
cat << EOF >> /etc/ansible/ansible.cfg
[defaults]
roles_path=/home/vagrant/roles
EOF
exit
cd

# Install roles. Seems ansible 2.3 cannot clone directly from a git repo
ansible-galaxy install https://github.com/rhysmeister/common/archive/common.tar.gz
ansible-galaxy install https://github.com/rhysmeister/mariadb10.3/archive/mariadb10.3.tar.gz
ansible-galaxy install https://github.com/rhysmeister/nginx/archive/nginx.tar.gz
ansible-galaxy install https://github.com/rhysmeister/awx/archive/awx.tar.gz

# TODO: Create playbook to run the above roles against the other hosts
```

rhysmeister.common      - Common Linux stuff.
rhysmeister.nginx       - Nginx webserver with php.
rhysmeister.mariadb     - MariaDB role.
rhysmeister.awx         - AWX (Open source version of Ansible Tower role).
