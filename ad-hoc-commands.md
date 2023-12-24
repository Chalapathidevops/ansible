# Ad-hoc commands

* An Ansible ad hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes.
* Ad-hoc commands are quick and easy, but they are not reusable.
* Ad-hoc commands in Ansible allow us to run quick, one-off tasks directly from the command line without the need for writing playbooks.
* Ad-hoc commands accepts only one set of arguments for a single module (for this we need to use ansible playbooks)

**Syntax:**

`ansible <host-pattern> -i <inventory> -m <module> -a "<module-options>"`

**Examples:**

* To create a file on node server `ansible -i inventory all -m "shell" -a "touch test2"`
* To list files and directory `ansible -i inventory all -m command -a "ls -ltr"`
* Copy file `ansible -i inventory all -m copy -a "src=/root/ansible/example.txt dest=/root"`
* Give permissions to the file `ansible -i inventory all -m copy -a "src=/root/ansible/example.txt dest=/root mode=400"`

**Shell vs Command** modules
* `command` module executes commands directly without shell processing, we can't use special characters like `&&`
  * Example: `ansible -i inventory all -m command -a "df -kh && tree-kh"` It will through error
* `shell` module executes commands through the system's default shell (like /bin/sh on many systems).
  * Example: `ansible -i inventory all -m shell -a "df -kh && tree-kh"`
 
**File** module
* The file module in Ansible is used to manage files and file properties on remote hosts. It allows you to create, modify, or delete files, as well as set file permissions, ownership, and other attributes.<br>
  `ansible -i inventory all -m file -a "dest=/root/example1.txt mode=400 state=touch"`
* **Delete file** `ansible -i inventory all -m file -a "dest=/root/example1.txt mode=400 state=absent"`
* **Create directory** `ansible -i inventory all -m file -a "dest=/root/test-folder state=directory"'
* **Remove directory** `ansible -i inventory all -m file -a "dest=/root/test-folder state=absent"`

