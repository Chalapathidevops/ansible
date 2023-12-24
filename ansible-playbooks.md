# Ansible Playbooks

* Ansible Playbooks are lists of tasks that automatically execute for specified inventory or groups of hosts. One or more Ansible tasks can be combined to make a play an ordered grouping of tasks mapped to specific hosts—and tasks are executed in the order in which they are written.
* Ansible Playbooks offer a repeatable, reusable, simple configuration management and multi-machine deployment system, one that is well suited to deploying complex applications.
* If you need to execute a task with Ansible more than once, write a playbook and put it under source control. Then you can use the playbook to push out new configuration or confirm the configuration of remote systems.

**Example:**

1. Create a file using ansible playbook

```bash
---
- name: Create a sample file
  hosts: all  # Execute for all the nodes
  tasks:
  - name: creating file
    file: 
      path: /root/sample.txt
      state: touch


root@ip-172-31-11-90:~/ansible# ansible-playbook -i inventory create_file.yaml

PLAY [Create a sample file] *************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************
ok: [172.31.46.176]

TASK [creating file] ********************************************************************************************************************
changed: [172.31.46.176]

PLAY RECAP ******************************************************************************************************************************
172.31.46.176              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

2. Install nginx and start

```
---
- name: Install nginx
  hosts: all
  become: true
  tasks:
  - name: Install nginx
    apt: 
      name: nginx
      state: present
  - name: Start nginx
    service:
      name: nginx
      state: started


root@ip-172-31-11-90:~/ansible# ansible-playbook -i inventory install_nginx.yaml

PLAY [Install nginx] ********************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************
ok: [172.31.46.176]

TASK [Install nginx] ********************************************************************************************************************
changed: [172.31.46.176]

TASK [Start nginx] **********************************************************************************************************************
ok: [172.31.46.176]

PLAY RECAP ******************************************************************************************************************************
172.31.46.176              : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

## Loops

* Loops in Ansible playbooks allow to perform tasks iteratively, repeating the same action for each item in a list or dictionary.
* Ansible supports different types of loops, such as `with_items`, `loop`, and `with_dict`.

1. Create multiple files using loops
   
```bash
---
- name: Create multiple files
  hosts: all
  become: true
  tasks:
  - name: Creating files
    file:
      path: "{{ item }}"
      state: touch
    with_items:
    - file1.txt
    - file2.txt
    - file3.txt
```
2. Create multiple files with different file permissions
```
---
- name: Create multiple files with different file permissions
  hosts: all
  become: true
  tasks:
  - name: Creating files with different permissions
    file:
      path: "{{ item.location }}"
      state: touch
      mode: "{{ item.mode }}"
    with_items:
    - { location: 'file1.txt', mode: '400' }
    - { location: 'file2.txt', mode: '777' }
    - { location: 'file3.txt', mode: '766' }
```

3. Install multiple packages using loops
```
---
- name: Install multiple packages with 'loop'
  hosts: all
  become: true
  tasks:
  - name: Install packages using loop
    apt:
      name: "{{ item }}"
      state: present
    loop:
      - nginx
      - mysql-server
      - python3-pip
      - git

```

## Tags
Tags in Ansible playbooks allow you to selectively execute specific tasks or groups of tasks rather than running the entire playbook. They provide a way to categorize tasks and control which tasks to execute based on the tags specified during playbook execution.

```
---
- name: Install nginx
  hosts: all
  become: true
  tasks:
  - name: Install nginx
    apt: 
      name: nginx
      state: present
    tags: install  
  - name: Start nginx
    service:
      name: nginx
      state: started
    tags: start
```

* Based on input playbook will run if tag is install, then it will install the nginx ans will skip start task
  
**Commands**

```bash
  ansible-playbook -i inventory install_nginx.yaml --tags "install"

  ## Skipping Tasks

  ansible-playbook -i inventory install_nginx.yaml --skip-tags "install"
```

* To list all the tags `ansible-playbook -i inventory install_nginx.yaml --list-tags`
```
playbook: install_nginx.yaml

  play #1 (all): Install nginx  TAGS: []
      TASK TAGS: [install, start]
```

