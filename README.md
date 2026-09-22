# Ansible LAMP Roles Lab

This project deploys a LAMP environment and the MariaDB Developers PHP Quickstart application using two reusable Ansible roles.

- `roles/lamp`: installs and configures HTTPD, MariaDB, PHP/PHP-FRM, the HTTPD template, and the database application credentials.
- `roles/app`: deploys `/var/www/html/index.php` with `phpinfo()`, clones the required `mariadb-developers/php-quickstart` repo, imports its schema once, deploys the PHP source under `/var/www/html/app`, grants database access, and renders the application's `config.php` from a template.
- `site.yml`: coordinates the two roles and reboots the managed server after deployment.

The MariaDB application password is prompted for at runtime and is not stored in this project.

