# RedhatAnsible

An Red Hat Enterprise 3.7 setup with Ansible 2.3 VM intended for the Red Hat Certified Specialist in Ansible Automation exam (EX407)

The following VMs will be created

ansible - Ansible control host.
web1 - nginx web server.
web2 - nginx web server.
db1 - database server.
db2 - database server.
tower - Ansible Tower / AWX.

The Plan
===========

This is intend to be a run-through of a bunch of Ansible stuff to prepare for the EX407 exam.

You can replace roles and vm with your own if desired.

rhysmeister.nginx - Nginx webserver with php.
rhysmeister.db    - MySQL role
rhysmeister.awx   - AWX (Open source version of Ansible Tower role)
