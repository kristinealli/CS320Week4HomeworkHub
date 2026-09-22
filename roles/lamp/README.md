Role Name
=========

lamp

Installs and configures an Apache, MariaDB, and PHP/PHP-FPM stack on a
Red Hat-family Linux host. The role creates the web root, starts and enables
the services, creates the application database user, and deploys the HTTPD
configuration used by the application.

Requirements
------------

* A Red Hat-family host with `dnf` package management.
* Ansible privilege escalation must be available because the role installs
  packages, manages services, writes under `/etc/httpd`, and creates a MariaDB
  user.
* Install the collection dependencies before running the playbook:

      ansible-galaxy collection install -r requirements.yml

The role uses the `community.mysql` collection and expects the managed host to
provide a local MariaDB Unix socket at `mysql_socket` after MariaDB starts.

Role Variables
--------------

The following variables are defined in `defaults/main.yml` and can be
overridden when the role is included:

| `lamp_packages` | `httpd`, `mariadb`, `mariadb-server`, `php`, `php-fpm`, `php-mysqlnd`, `python3-PyMySQL`, `git` | Packages installed by the role. |
| `httpd_service` | `httpd` | Apache service name. |
| `mariadb_service` | `mariadb` | MariaDB service name. |
| `php_service` | `php-fpm` | PHP-FPM service name. |
| `http_config_path` | `/etc/httpd/conf/httpd.conf` | HTTPD configuration destination. |
| `mysql_socket` | `/var/lib/mysql/mysql.sock` | MariaDB Unix socket used by `community.mysql`. |
| `web_user` / `web_group` | `apache` / `apache` | Web server account and group. |
| `httpd_server_root` | `/etc/httpd` | Apache server root. |
| `httpd_listen_port` | `80` | Port on which Apache listens. |
| `httpd_server_admin` | `root@localhost` | Apache administrator address. |
| `httpd_document_root` | `{{ document_root }}` | Apache document root. |
| `httpd_directory_index` | `index.php index.html` | Default index files. |
| `httpd_user` / `httpd_group` | `apache` / `apache` | User and group used by Apache. |

The role also consumes these variables from the playbook or shared variables:

* `document_root`: Base web root, normally `/var/www/html`.
* `app_db_user`: MariaDB user to create.
* `app_db_password`: Password for the MariaDB user. Supply it with a prompt or
  Ansible Vault; do not commit it to source control.

The role does not create the application database itself. The `app` role
imports the application schema after this role creates the database user.

Dependencies
------------

* Ansible collection: `community.mysql`.
* No external Ansible role dependencies.
* When used by this project, run `lamp` before `app` so the services, socket,
  document root, and MariaDB application user are ready.

Example Playbook
----------------

    - name: Deploy the LAMP stack
      hosts: webservers
      become: true
      vars:
        document_root: /var/www/html
        app_db_user: rolodex_user
      vars_prompt:
        - name: app_db_password
          prompt: "Enter the MariaDB password for the app user"
          private: true
      roles:
        - lamp
NOTE:The application password should be supplied through the prompt.

To deploy the complete project, place the `app` role after `lamp`:

    roles:
      - lamp
      - app

License
-------

Not applicable, this project is for BMCC Week 3 CS320 Lab.

Author Information
------------------

Kristine Johnson
