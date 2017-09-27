# Ansible Role: PHP on CentOS 7

Installs PHP **5.4** or **5.5** or **5.6** or **7.0** or **7.1** on CentOS 7 using `base` or `ius` or `webtatic` or `remi` repositories.

## Requirements

Tested CentOS 7

## Get started

If you came here and you have no idea where to start and **the only think you worry is PHP 5.6 on Centos 7**  select the following commands and paste it on your terminal.

    # Install EPEL repository (need it for ansible)
    sudo yum install -y epel-release
    # Install Ansible
    sudo yum install -y ansible
    # Install git
    sudo yum install -y git
    # Create Ansible hosts file mapped on localhost
    sudo echo "localhost ansible_connection=local" >> /etc/ansible/hosts
    # Create you first playbook
    mkdir ~/ansible-playbook
    echo "---

    - hosts: all
      become: yes
      roles:
        - ansible-role-php" > ~/ansible-playbook/php-playbook.yml
    # Clone this project (it's an ansible role)
    git clone https://github.com/tovletoglou/ansible-role-php.git ~/ansible-playbook/roles/ansible-role-php
    # Run the playbook
    ansible-playbook ~/ansible-playbook/php-playbook.yml
    # Test PHP
    php -v
    # Enjoy

## Role Variables

Available variables are listed below, along with default values `defaults/main.yml`

**Remove any php version before install** (true | false)

    remove_php: true

**Update php every time you run this role** (present | latest)

    yum_state: latest

**Define if you are using a web server** (true | false)

    php_webserver: true

**Define your web server** (httpd | nginx).

    webserver: httpd

**Select php version** (php | php55 | php56 | php70 | php71)

| package name | version | base | ius | webtatic | remi |
| ---          | ---     | ---  | --- | ---      | ---  |
| php          | 5.4.x   | ok   |     |          | ok   |
| php55        | 5.5.x   |      | ok  | ok       | ok   |
| php56        | 5.6.x   |      | ok  | ok       | ok   |
| php70        | 7.0.x   |      | ok  | ok       | ok   |
| php71        | 7.1.x   |      |     |          | ok   |

Be extra careful when you choose php version.
Not every repository have the corresponding php version. Check the table above.

    php_version: php56

**Choose repository** (base | ius | webtatic | remi)

ius and remi needs epel repo, epel will be installed automatically if you choose one of these two.
If you are using php (php 5.4) use the base repo, otherwise I suggest the isu repository.

    repository: ius

Add or remove php modules (do not include php itself)

    php_modules:
      - cli
      - common
      - devel
      - fpm
      - gd
      - ldap
      - mbstring
      - mcrypt
      - mysqlnd
      - opcache
      - pdo
      - pear
      - process
      - xml

**Create CUSTOM.php.ini** (true | false)

    create_custom_php_ini: true

**Give the name you want for the CUSTOM.php.ini** (string)

Edit the php custom configuration in the `templates/custom.php.ini.tpl`

    custom_php_ini: custom

**Create DOMAIN/phpinfo.php test page** (true | false)

    create_phpinfo: true

**Web server folder** (string, followed by '/')

    webserver_folder: /var/www/html/

## Dependencies

None, if you don't have a web server (httpd, nginx) change the `defaults/main.yml`  `php_webserver: true` to `false`

## Example Playbook

Installed with galaxy.ansible `ansible-galaxy install tovletoglou.php`

    ---

    - hosts: all
      roles:
        - { role: tovletoglou.php }

Installed as a playbook git submodule  `git clone https://github.com/tovletoglou/ansible-role-php.git playbook/roles/ansible-role-php`

    ---

    - hosts: all
      roles:
        - { role: ansible-role-php }

Sending custom vars

    ---

    - hosts: all
      roles:
        - { role: tovletoglou.php }
      vars:
        remove_php: true
        php_modules:
          - cli
          - common

## License

MIT

## Author Information

Apostolos Tovletoglou [ansible-role-php](https://github.com/tovletoglou/ansible-role-php)
