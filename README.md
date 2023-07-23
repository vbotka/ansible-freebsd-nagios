# freebsd_nagios

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_nagios)[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-nagios.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-nagios)


## Table of Contents
* [Introduction](#Introduction)
* [Requirements](#Requirements)
* [Recommended](#Recommended)
* [Variables](#Variables)
* [Workflow](#Workflow)
* [References](#References)
* [License](#License)
* [Author Information](#Author-Information)


## <a name="Introduction"></a>Introduction

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_nagios/) FreeBSD. Configure Nagios.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-nagios/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


## <a name="Requirements"></a>Requirements

### Collections

- community.general

### Recommended

- Role [vbotka.certificate](https://galaxy.ansible.com/vbotka/certificate/)
- Role [vbotka.apache](https://galaxy.ansible.com/vbotka/apache/)


## <a name="Recommended"></a>Recommended

- [Apache](https://galaxy.ansible.com/vbotka/apache/)
- [Certificate](https://galaxy.ansible.com/vbotka/certificate/)


## <a name="Variables"></a>Variables

Review the defaults and examples in vars.


## <a name="Workflow"></a>Workflow

1) Change shell to /bin/sh

```
shell> ansible host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod admin -s /bin/sh'
```

2) Install the role and collections

```
shell> ansible-galaxy role install vbotka.freebsd_nagios
shell> ansible-galaxy collection install community.general
```

3) Fit variables, e.g. in vars/main.yml

```
shell> editor vbotka.freebsd_nagios/vars/main.yml
```

4) Create and run the playbook

```
shell> cat freebsd-nagios.yml
- hosts: nagios.example.com
  roles:
    - vbotka.freebsd_nagios
    
shell> ansible-playbook freebsd-nagios.yml
```

5) Create [certificates](https://galaxy.ansible.com/vbotka/certificate/)

```
certificate_self_signed:
  - {CN: 'nagios.example.com',
     private: 'nagios.example.com.key',
     csr: 'nagios.example.com.csr',
     crt: 'nagios.example.com.crt'}
```

6) Configure [Apache](https://galaxy.ansible.com/vbotka/apache/)

```
apache_vhost:
  - ServerName: 'nagios.example.com'
    DocumentRoot: '/usr/local/www/nagios'
    SSLCertificateFile: '/usr/local/etc/ssl/certs/nagios.example.com.crt'
    SSLCertificateKeyFile: '/usr/local/etc/ssl/private/nagios.example.com.key'

apache_directory_blocks:
  - Directory: '/usr/local/www/nagios/'
    Includefile: 'usr-local-www-nagios.conf'
    Conf:
      - 'AllowOverride AuthConfig'
      - 'AuthType Basic'
      - 'AuthBasicProvider file'
      - 'AuthUserFile /usr/local/etc/nagios/htpasswd.users'
      - 'AuthName MySite'
      - 'Require valid-user'
      - 'Order allow,deny'
      - 'Allow from 10.1.0.11'
      - 'DirectoryIndex index.php index.html index.htm'
      - 'AddType application/x-httpd-php .php'
      - 'AddType application/x-httpd-php-source .phps'
      - 'Options Indexes FollowSymLinks'
      - 'Require all granted'
      - 'php_flag engine on'
      - 'php_admin_value open_basedir /usr/local/www/nagios/:/var/spool/nagios/'
  - Directory: '/usr/local/www/nagios/cgi-bin'
    Includefile: 'usr-local-www-nagios-cgibin.conf'
    Conf:
      - 'Options ExecCGI'

apache_alias:
  - 'ScriptAlias /nagios/cgi-bin/ /usr/local/www/nagios/cgi-bin/'
  - 'Alias /nagios/ /usr/local/www/nagios/'

apache_httpd_conf_modules:
  - {module: 'cgi_module', mod: 'mod_cgi.so', present: true}
  - {module: 'cgid_module', mod: 'mod_cgid.so', present: true}
```

7) Create password for nagiosadmin

```
shell> htpasswd -c /usr/local/etc/nagios/htpasswd.users nagiosadmin
```


## <a name="References"></a>References

- [Nagios Documentation](https://assets.nagios.com/downloads/nagioscore/docs/)
- [Apache Tutorial: Dynamic Content with CGI](https://httpd.apache.org/docs/2.4/howto/cgi.html)
- [Apache HowTo: Authentication and Authorization](https://httpd.apache.org/docs/2.4/howto/auth.html)
- [Apache HowTo: Access Control](https://httpd.apache.org/docs/2.4/howto/access.html)


## <a name="License"></a>License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## <a name="Author-Information"></a>Author Information

[Vladimir Botka](https://botka.info)
