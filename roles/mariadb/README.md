Role Name
=========

A basic MariaDB setup.

Role Variables
--------------

mariadb_repo: Dictionary for MariaDB yum repo.
mariadb_packages: MariaDB packages to install.
mariadb_package_state: Installation state of MariaDB packages.
mariadb_port: Port for MariaDB in format NNNN/tcp
required_packages: Additional packages required for the role.
databases: MariaDB databases to create.

These variables are stored using ansible-vault.

super_secret_vault_password: MariaDB root password
secret_ro: MariaDB user read-only password
secret_rw:  MariaDB user read-write password

Author Information
------------------

Rhys Campbell <rhys.james.campbell@googlemail.com> http://github.com/rhysmeister
