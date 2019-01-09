freebsd_nagios
==============

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-nagios.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-nagios)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_nagios/) FreeBSD. Configure Nagios.


Requirements
------------

No requiremenst.


Recommended
-----------

- [Apache](https://galaxy.ansible.com/vbotka/apache/)
- [Certificate](https://galaxy.ansible.com/vbotka/certificate/)


Variables
---------

TBD. Review the defaults and examples in vars.


Workflow
--------

1) Change shell to /bin/sh.

```
# ansible host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod admin -s /bin/sh'
```

2) Install role.

```
# ansible-galaxy install vbotka.freebsd_nagios
```

3) Fit variables.

```
# editor vbotka.freebsd_nagios/vars/main.yml
```

4) Create and run the playbook.

```
# cat freebsd-nagios.yml
- hosts: nagios.example.com
  roles:
    - vbotka.freebsd_nagios
    
# ansible-playbook freebsd-nagios.yml
```

5) Create [certificates](https://galaxy.ansible.com/vbotka/certificate/).

```
certificate_self_signed:
  - { CN: "nagios.example.com",
      private: "nagios.example.com.key",
      csr: "nagios.example.com.csr",
      crt: "nagios.example.com.crt" }
```

6) Configure [Apache](https://galaxy.ansible.com/vbotka/apache/).

```
apache_vhost:
  - ServerName: "nagios.example.com"
    DocumentRoot: "/usr/local/www/nagios"
    SSLCertificateFile: "/usr/local/etc/ssl/certs/nagios.example.com.crt"
    SSLCertificateKeyFile: "/usr/local/etc/ssl/private/nagios.example.com.key"

apache_directory_blocks:
  - Directory: "/usr/local/www/nagios/"
    Includefile: "usr-local-www-nagios.conf"
    Conf:
      - "DirectoryIndex index.php index.html index.htm"
      - "AddType application/x-httpd-php .php"
      - "AddType application/x-httpd-php-source .phps"
      - "Options Indexes FollowSymLinks"
      - "AllowOverride All"
      - "Require all granted"
      - "php_flag engine on"
      - "php_admin_value open_basedir /usr/local/www/nagios/:/var/spool/nagios/"
  - Directory: "/usr/local/www/nagios/cgi-bin"
    Includefile: "usr-local-www-nagios-cgibin.conf"
    Conf:
      - "Options ExecCGI"

apache_alias:
  - "ScriptAlias /nagios/cgi-bin/ /usr/local/www/nagios/cgi-bin/"
  - "Alias /nagios/ /usr/local/www/nagios/"
```

References
----------

- [Nagios Documentation](https://assets.nagios.com/downloads/nagioscore/docs/)
- [Apache Tutorial: Dynamic Content with CGI](https://httpd.apache.org/docs/2.4/howto/cgi.html)

License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


Author Information
------------------

[Vladimir Botka](https://botka.link)
