Role Name
=========

Installs Tomcat WebServer on RHEL/CentOS

Requirements
------------

Requires JVM.

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

    java_version: 1.8.0

    tomcat_version: 8.5.42

    tomcat_http_port: 8080

    tomcat_https_port: 8443

    tomcat_admin_username: admin

    tomcat_admin_password: admin


Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: tomcat_server
  roles:
     - role: andyneeb.tomcat
       become: yes
```

License
-------

Apache 2.0

Author Information
------------------

This role was created by Andreas Neeb in 2019.

```shell
   https://galaxy.ansible.com/andyneeb/tomcat
   ansible-galaxy install andyneeb.tomcat
   cd .ansible/
   cd roles/
   cd andyneeb.tomcat/
   vi README.md
   cd ansible-course/
   cp -r ~/.ansible/roles/andyneeb.tomcat/ .
   ssh-copy-id centos@51.254.227.50
   ansible all -m ping -i inventory_children

```


## Syntax
```yaml
- name: restart apache
  service: name=apache state=restarted
```

```yaml
- name: restart apache
  service:
    name: apache
    state: restarted
```



