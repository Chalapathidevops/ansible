# Ansible Roles

* Role is like a collection of tasks, configurations, files, and templates organized together to perform a specific function or manage a certain aspect of system configuration. Roles help in structuring and organizing your playbook into reusable components, making your automation more modular and easier to manage.
* Think of a role as a set of instructions and resources bundled together for a particular job. For example, if you have tasks to set up a web server, including installing software, configuring files, and starting services, you can organize these tasks into a role called webserver.

* Use the ansible-galaxy command to create a role directory structure `ansible-galaxy init webserver_role`
```bash
  webserver_role/
├── README.md
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
└── vars
    └── main.yml
```

* Inside the tasks/main.yml file, define the tasks required for setting up the web server:
```
---
- name: Install web server packages
  ansible.builtin.package:
    name: "{{ item }}"
    state: present
  loop:
    - apache2
    - nginx
    - php
```
* Define variables in vars/main.yml:
```
---
web_server_port: 80
```
* Handlers in handlers/main.yml define actions to be taken when notified by tasks:
```
---
- name: Restart web server
  ansible.builtin.service:
    name: "{{ web_server_service }}"
    state: restarted
```

* In your playbook, include the role:
```
---
- name: Setup Web Server
  hosts: webservers
  roles:
    - webserver_role
```
* To execute the playbook that uses the webserver_role:
`ansible-playbook -i inventory role_example.yml`

