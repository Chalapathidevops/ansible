# What is Ansible
* Ansible is a popular open-source automation tool used for configuring and managing computer systems. In simple terms, it allows you to automate various tasks like setting up servers, installing software, deploying applications, and managing configurations across multiple machines without needing to manually perform each task.
* Using Ansible, we can define tasks and configurations in a simple human-readable language (YAML), called playbooks.
* These playbooks contain instructions on what actions to take on different systems, and Ansible then executes these tasks remotely via SSH, making it easier to manage and maintain infrastructure, whether it's a handful of servers or a large network of computers.

![image](https://github.com/Chalapathidevops/ansible/assets/145283206/a2e0c167-d0c8-48be-8be0-eec1c992be48)

## Difference between Ansible and Puppet

| Ansible    | Puppet |
| -------- | ------- |
| Agentless. Ansible operates over SSH, relying on existing SSH infrastructure to communicate with remote servers, without needing any agent installation on managed nodes.  | Agent-based. Puppet requires a Puppet agent to be installed on managed nodes, which communicates with a central Puppet server.    |
| Follows a push-based architecture. The control machine executes tasks on remote machines by pushing configurations and commands through SSH connections. | Follows a pull-based architecture. Managed nodes pull configurations from the central Puppet server at specified intervals.     |
| Uses YAML (Yet Another Markup Language) for defining playbooks, which are human-readable files describing desired system states and tasks.    | Uses its own declarative language (Puppet DSL) for defining configurations. It focuses on describing the desired state of the system rather than the sequence of steps to achieve it.    |
| Well-suited for managing large-scale infrastructures due to its agentless nature and parallel execution capabilities. | Also capable of managing large infrastructures, but the agent-based approach might introduce additional overhead in maintaining and scaling the agent installations. |
| Known for its large and active community, extensive module library, and integration with various cloud platforms and third-party tools. | Also has a strong community and ecosystem with avariety of modules and integrations, but it might be relatively smaller compared to Ansible's. |

## Ansible Inventory: Dynamic vs. Static

**Static Inventory:**
* The static inventory is the most basic type of inventory that you can create in Ansible. We can manually create this file, which will include the group name and all the hosts that you want to run the Ansible playbooks against.
* Static inventories are easy to understand and use, especially in smaller environments that has a handful of machines that don’t change frequently.
* However, if you have a large or growing number of machines that you will be managing through Ansible, it might be wise to incorporate a Dynamic Inventory file in your setup, this will save your team tons of time and will automate the process of managing your inventory files.
* Here is a sample inventory file:
```
[ubuntu]
db01.azure.com
10.1.1.10 #adding a IP here since you are also able to add IP address instead of DNS name

[centos]
webserver01.azure.com
webserver02.azure.com

[kali]
webserver03.azure.com
webserver04.azure.com
```

**Dynamic Inventory:**
* Dynamic Inventory comes in handy when your Ansible inventory fluctuates overs time, with different management nodes spinning up and shutting down frequently. Dynamic Inventory has two ways of retrieving the management nodes, inventory scripts and plugins.
* Inventory Scripts are executed during runtime to generate a list of hosts and is able to query virtually any source of information about the management nodes. In context of Azure resources, the script will be able to leverage Azure’s API to discover the resources in real time. However, the process of setting up these inventory scripts can get a bit complex compared to using Inventory Plugins.
* Inventory Plugins takes scripts one step further, it provides are more efficient and reliable way to manage your Azure resources using Ansible. It interacts directly with Azure APIs, querying them for the current state of the Azure resources and returns it in a format that Ansible can import. The Azure plugin is able to dynamically query and group the resources based on any specific Azure property such as tags, location, resource group, subscription etc. This is the key difference between dynamic and static inventory files. The Azure plugin also supports filters, which can help narrow down the list of resources based on granular details.
