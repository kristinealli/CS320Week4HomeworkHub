Role Name
=========

app

Deploys the MariaDB Developers PHP Quickstart application and a PHP info page
to a host with a configured Apache, PHP, and MariaDB stack. The role clones the
quickstart repository, imports its schema when the `contacts` table is absent,
grants the application database user access, copies the PHP source, and
renders its database connection configuration.

Requirements
------------

* A Red Hat-family host with Apache, PHP, PHP-MySQL support, MariaDB, and Git
  installed. The `lamp` role in this project provides these prerequisites.
* MariaDB must be running and accessible through `mysql_socket`.
* The managed host must be able to reach `app_repo`.
* Install the collection dependencies before running the playbook:

      ansible-galaxy collection install -r requirements.yml


Role Variables
--------------

Variables defined by this role in `defaults/main.yml`:

| `phpinfo_page` | `{{ document_root }}/index.php` | Destination for the PHP info page. |
| `quickstart_schema` | `{{ app_clone_path }}/schema.sql` | Schema file imported into MariaDB. |
| `quickstart_source` | `{{ app_clone_path }}/src` | Cloned application source directory. |

Variables shared with the LAMP configuration:

* `document_root`: Apache document root, normally `/var/www/html`.
* `app_path`: Application destination, normally `/var/www/html/app`.
* `app_repo` and `app_clone_path`: Git repository and temporary clone path.
* `app_db_name`, `app_db_user`, `app_db_password`, and `app_db_host`: Database
  connection settings. The password should be supplied securely, such as with
  `vars_prompt` or Ansible Vault.
* `mysql_socket`: MariaDB Unix socket path.
* `web_user` and `web_group`: Owner and group for deployed application files.

The `lamp` role must create the database user before this role runs. In the
provided playbook, `app_db_password` is prompted for at runtime.

Dependencies
------------

* Ansible collection: `community.mysql`.
* Project role dependency: run the `lamp` role first so Apache, PHP, MariaDB,
  the socket, and the application database user are available.

Example Playbook
----------------

The following example shows the required role order. 

    - name: Deploy the LAMP application
      hosts: webservers
      become: true
      vars_prompt:
        - name: app_db_password
          prompt: "Enter the MariaDB password for the app user"
          private: true
      roles:
        - lamp
        - app

License
-------

Not applicable, this project is for BMCC Week 3 CS320 Lab.

Author Information
------------------

Kristine Johnson
