# Ansible Vault

Ansible Vault is a tool that helps securely store sensitive information like passwords, API keys, or any other confidential data used in Ansible playbooks. It encrypts these sensitive details to protect them from unauthorized access, allowing you to safely store and use them within your automation code.

* Creating an Encrypted File: `ansible-vault create secrets.yml` It will ask Vault password, enter the password
```
root@ip-172-31-11-90:~/ansible# ansible-vault create secrets.yml
New Vault password:
Confirm New Vault password:
```

* Editing an Encrypted File: `ansible-vault edit secrets.yml`
* To run the playbook `ansible-playbook -i inventory --ask-vault-pass secrets.yml` It will ask password and you should enter it.
* We can apply to the variable file, instead of encrypting entair playbook we can use seperate file for that
* Also, we can store all passwords seperate file and use it instead of giving `--ask-vault-pass` we can use `vault_id`

**Commands:**
* To encrypt the playbook: `ansible-vault encrypt secrets.yml`
* To decrypt or remove protection the playbook: `ansible-vault decrypt secrets.yml`
* To view: `ansible-vault view secrets.yml`
* To change the password: `ansible-vault rekey secrets.yml`
