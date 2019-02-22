# Ansible tomcat_application role

This is an [Ansible](http://www.ansible.com) role that deploys applications in tomcat server instances configuring also datasources and directories.

## Requirements

[Ansible 2.7+](http://docs.ansible.com/ansible/latest/intro_installation.html)

## Role Variables

A list of all the default variables for this role is available in `defaults/main.yml`. The role setups the following facts:

- tomcat_application_server_instances: list of dicts with tomcat instances info. Each dict contains the following info: `name`, `service`, `base`, `home`, `user`, `group`, `autodeploy` and `jsvc`.

- tomcat_application_datasources_templated: application datasources after template application.

## Dependencies

- [amtega.check_platform](https://galaxy.ansible.com/amtega/check_platform)
- [amtega.artifact](https://galaxy.ansible.com/amtega/artifact)

## Example Playbook

This is an example playbook:

```yaml
---

- hosts: all
  roles:
    - role: tomcat_application
      tomcat_application_name: sample
      tomcat_application_instances:
        - tomcat@server1
        - tomcat@server2
      tomcat_application_artifacts:
        - url: https://tomcat.apache.org/tomcat-8.0-doc/appdev/sample/sample.war
          dest: webapps
          timeout: 60
          validate_certs: false        
      tomcat_application_dirs:
        - "config/wanda"
        - "log/wanda"
        - "cert/wanda"
        - "data/wanda"
        - "resource/wanda"
      tomcat_application_datasources:
        - name: "jdbc/wandaDataSource"
          auth: Container
          type: acme.jdbc.pool.AcmeDataSource
          factory: acme.jdbc.pool.AcmeDataSourceFactory
          driverClassName: acme.jdbc.AcmeDriver
          maxTotal: 100
          maxIdle: 30
          maxWaitMilli: 10000
          url: dbc:acme:@DATABASE
          user: app
          password: app_password        
```

## Testing

Tests are based on docker containers. You can setup docker engine quickly using the playbook `files/setup.yml` available in the role [amtega.docker_engine](https://galaxy.ansible.com/amtega/docker_engine).

Once you have docker, you can run the tests with the following commands:

```shell
$ cd amtega.tomcat_application/tests
$ ansible-playbook main.yml
```

## License

Copyright (C) 2018 AMTEGA - Xunta de Galicia

This role is free software: you can redistribute it and/or modify
it under the terms of:
GNU General Public License version 3, or (at your option) any later version;
or the European Union Public License, either Version 1.2 or – as soon
they will be approved by the European Commission ­subsequent versions of
the EUPL;

This role is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details or European Union Public License for more details.

## Author Information

- Juan Antonio Valiño García.
