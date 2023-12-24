# Ansible Installation steps

Step-1: Create EC2 instance (name: ansible-server), and login to server -> run below commands 
  ```
  apt update
  apt install ansible
  ansible --version
```
Step-2: I just copied the public ip of ansible server to the authorized_keys
  * Create one more instance (Name: node)
  * For **password less authentication** do `ssh-keygen` in ansible-server, it will create public and private key 
      ```
         cd /root/.ssh/
         cat id_rsa.pub     # Copy the key
      ```
      
      * Login to the node server and do `ssh-keygen`
        `cd /root/.ssh/`
        Edit **authorized_keys** and paste key (which is copied above)
  * Now, try to connect to node server using ssh <privateID>

  ***DONE!!***
  * Create inventory file **inventory** in ansible server and add private ips of node servers
   ```
    root@ip-172-31-11-90:~/ansible# cat inventory
    172.00.00.100
  ```
  * Try to execute ansible command
  `ansible -i inventory all -m "shell" -a "touch test1"` and see the file is created in node server.
<br><br>

**Ref https://github.com/ansible/ansible-examples/tree/master** <br>
**Ref https://github.com/ansible/ansible-examples**
