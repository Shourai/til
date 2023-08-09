# Using ansible inventory in python

https://www.yaklin.ca/2021/08/05/ansible-inventory-in-python.html

With the various methods for performing network automation, one of the challenging aspects to consider is inventory management. One of the tools available to us is Ansible which expects an inventory file in YAML format with specific variable or to use a dynamic inventory. But Ansible doesn't solve all automation use-cases. I have used Ansible for configuration management, but I have also used many different Python scripts for generating reports and performing complex operations that seemed easier to implement directly in Python than in Ansible. There is no 'one size fits all' solution to network automation.

While there are systems out there, such as [NetBox](https://netbox.readthedocs.io/en/stable/), as a source of truth for inventory management, we don't always have the luxury of these tools. In this article I present a way to use Ansible inventory files directly in our Python scripts. While there are a few caveats (discussed below), this might be a solution that fits your use-case.

Requirements and Caveats
------------------------

The examples in this article use Ansible version 2.11.1 and Python 3.8.10. We will be using the [Ansible Python API](https://docs.ansible.com/ansible/latest/dev_guide/developing_api.html) for reading our inventory file to identify hosts and their variables.

This method works well if you are setting your group variables directly in your inventory file. So far I have not found a method that allows you to load your group_vars files in addition to your inventory file. Additionally, I have not found a method that will allow for loading Ansible Vault variables that are in these files. If I find a method to do this in the future I will be sure to provide an update! If you are aware of how to do this, I would love to hear from you!

Inventory File
--------------

This article will be using an inventory file named hosts.yaml, as shown below:

```
---
all:
  hosts:
    RouterA:
      ansible_host: 192.168.10.150
      syslog_servers:
        - 192.168.1.1
        - 192.168.1.2
    SwitchA:
      ansible_host: 192.168.10.151
  vars:
    syslog_servers:
      - 10.1.1.1
      - 10.1.1.2
  children:
    ios:
      hosts:
        RouterA:
    nxos:
      hosts:
        SwitchA:

```

We have two hosts (RouterA and SwitchA) and a three different groups (all, ios, nxos). Additionally, there is a variable named syslog_servers which applies two IP addresses (10.1.1.1 and 10.1.1.2) to all hosts, with that being overwritten for RouterA to two diffrent IP addresses (192.168.1.1 and 192.168.1.2).

Using the Ansible Python API
----------------------------

To read the hosts.yaml file with Python in a convenient fashion we can use the [Ansible Python API](https://docs.ansible.com/ansible/latest/dev_guide/developing_api.html). While there are many methods for use within the API (such as for running Playbooks), this blog post is specifically focused on reading the inventory file to identify hosts, their IP addresses, groups, and variables so that we can leverage this information in a custome Python script you may have.

We will be using three classes to read our inventory file; InventoryManager, DataLoader and VariableManager. You can import these as follows:

```
>>> from ansible.inventory.manager import InventoryManager
>>> from ansible.parsing.dataloader import DataLoader
>>> from ansible.vars.manager import VariableManager

```

We must first create an instance of DataLoader. This class is used to load and parse YAML or JSON data. We do not provide our inventory file directly to the DataLoader class, this will happen in the next step.

```
>>> dl = DataLoader()

```

We now pass the DataLoader object and the name of our inventory file to the InventoryManager class. This class has a method named **get_hosts()** that we can use to return all hosts or a subset of hosts based on a pattern. It returns a list of Ansible inventory host objects. Later we will provide these objects to the VariableManager class instance to retreive variables for a particular host. Below I demonstrate creating the InventoryManager instance and using **get_hosts()** as well as a few other methods.

```
>>> im = InventoryManager(loader=dl, sources=['hosts.yaml'])
# Retreive all hosts
>>> im.get_hosts()
[RouterA, SwitchA]
# Retreive just the 'ios' group hosts
>>> im.get_hosts(pattern='ios')
[RouterA]
# Retreive all hosts that start with Switch
>>> im.get_hosts(pattern='Switch*')
[SwitchA]
# List the groups that are identified in the inventory file
>>> im.list_groups()
['all', 'ios', 'nxos', 'ungrouped']

```

There are a few ways that you can retreive the variables associated with each host. If you know the specific host that you want variables for you could pass that host directly to the VariableManager instance, or if you need to work against a group of hosts you can iterate over the list which the **get_hosts()** method provides and pass each item to the VariableManager. I'll demonstrate a few of these below.

```
# Create an instance of the VariableManager, passing it our DataLoader and InventoryManager class instances
>>> vm = VariableManager(loader=dl, inventory=im)
# Pass a specific host object to the VariableManager
>>> my_host = im.get_host('RouterA')
>>> vm.get_vars(host=my_host)
{'syslog_servers': ['192.168.1.1', '192.168.1.2'], 'inventory_file': '/home/byaklin/ansible/inventory/hosts.yaml', 'inventory_dir': '/home/byaklin/ansible/inventory', 'ansible_host': '192.168.10.150', 'inventory_hostname': 'RouterA', 'inventory_hostname_short': 'RouterA', 'group_names': ['ios'], 'ansible_facts': {}, 'playbook_dir': '/home/byaklin/ansible/inventory', 'ansible_playbook_python': '/usr/bin/python3', 'ansible_config_file': None, 'groups': {'all': ['RouterA', 'SwitchA'], 'ungrouped': [], 'ios': ['RouterA'], 'nxos': ['SwitchA']}, 'omit': '__omit_place_holder__6c71c9ad93fbde3c2d7cd7336c994199d4a97678', 'ansible_version': 'Unknown'}

```

The above example shows retreiving the variables for a single host (RouterA). There are many different variables/keys, but if we were to use these within our Python script we would primarily be wanting to use the **ansible_host** key, configuration related keys that our custom script might need such as **syslog_servers** (ex. if we were performing connectivity tests to the syslog servers from our host, or configuring the syslog server but we would probably do that directly in an Ansible playbook in the first place), as well as any **groups** the host is associated with.

To iterate over all hosts and use their variables as part of a more complex script, we can use the returned list from the InventoryManager's **get_hosts()** method.

```
# Iterate over all hosts
>>> for host in im.get_hosts():
...     host_vars = vm.get_vars(host=host)
...     print(host_vars)
...
{'syslog_servers': ['192.168.1.1', '192.168.1.2'], 'inventory_file': '/home/byaklin/ansible/inventory/hosts.yaml', 'inventory_dir': '/home/byaklin/ansible/inventory', 'ansible_host': '192.168.10.150', 'inventory_hostname': 'RouterA', 'inventory_hostname_short': 'RouterA', 'group_names': ['ios'], 'ansible_facts': {}, 'playbook_dir': '/home/byaklin/ansible/inventory', 'ansible_playbook_python': '/usr/bin/python3', 'ansible_config_file': None, 'groups': {'all': ['RouterA', 'SwitchA'], 'ungrouped': [], 'ios': ['RouterA'], 'nxos': ['SwitchA']}, 'omit': '__omit_place_holder__6c71c9ad93fbde3c2d7cd7336c994199d4a97678', 'ansible_version': 'Unknown'}
{'syslog_servers': ['10.1.1.1', '10.1.1.2'], 'inventory_file': '/home/byaklin/ansible/inventory/hosts.yaml', 'inventory_dir': '/home/byaklin/ansible/inventory', 'ansible_host': '192.168.10.151', 'inventory_hostname': 'SwitchA', 'inventory_hostname_short': 'SwitchA', 'group_names': ['nxos'], 'ansible_facts': {}, 'playbook_dir': '/home/byaklin/ansible/inventory', 'ansible_playbook_python': '/usr/bin/python3', 'ansible_config_file': None, 'groups': {'all': ['RouterA', 'SwitchA'], 'ungrouped': [], 'ios': ['RouterA'], 'nxos': ['SwitchA']}, 'omit': '__omit_place_holder__6c71c9ad93fbde3c2d7cd7336c994199d4a97678', 'ansible_version': 'Unknown'}

```

In the above example you can see that by using the Ansible API, the VariableManager parses through the inventory file to identify which variables are used by which hosts. RouterA has syslog_servers 192.168.1.1 and 192.168.1.2, while SwitchA has syslg_servers 10.1.1.1 and 10.1.1.2. The order of precedence on how variables is identified by **get_vars()** can be seen in the docstring:

```
>>> help(im.get_vars)
get_vars(play=None, host=None, task=None, include_hostvars=True, include_delegate_to=True, use_cache=True, _hosts=None, _hosts_all=None, stage='task') method of ansible.vars.manager.VariableManager instance
    Returns the variables, with optional "context" given via the parameters
    for the play, host, and task (which could possibly result in different
    sets of variables being returned due to the additional context).

    The order of precedence is:
    - play->roles->get_default_vars (if there is a play context)
    - group_vars_files[host] (if there is a host context)
    - host_vars_files[host] (if there is a host context)
    - host->get_vars (if there is a host context)
    - fact_cache[host] (if there is a host context)
    - play vars (if there is a play context)
    - play vars_files (if there's no host context, ignore
      file names that cannot be templated)
    - task->get_vars (if there is a task context)
    - vars_cache[host] (if there is a host context)
    - extra vars
...

```

Finally, to iterate over a subset of hosts and obtain their variables, you can provide a pattern to **get_hosts()** as shown previously.

```
>>> for host in im.get_hosts(pattern='ios'):
...     host_vars = vm.get_vars(host=host)
...     print(host_vars)
...
{'syslog_servers': ['192.168.1.1', '192.168.1.2'], 'inventory_file': '/home/byaklin/ansible/inventory/hosts.yaml', 'inventory_dir': '/home/byaklin/ansible/inventory', 'ansible_host': '192.168.10.150', 'inventory_hostname': 'RouterA', 'inventory_hostname_short': 'RouterA', 'group_names': ['ios'], 'ansible_facts': {}, 'playbook_dir': '/home/byaklin/ansible/inventory', 'ansible_playbook_python': '/usr/bin/python3', 'ansible_config_file': None, 'groups': {'all': ['RouterA', 'SwitchA'], 'ungrouped': [], 'ios': ['RouterA'], 'nxos': ['SwitchA']}, 'omit': '__omit_place_holder__6c71c9ad93fbde3c2d7cd7336c994199d4a97678', 'ansible_version': 'Unknown'}

```

TLDR
----

Using the Ansible Python API can be a helpful tool allowing you to use your Ansible inventory files in both Ansible playbooks and Python scripts (don't forget to read the Requirements and Caveats section above). It is quite easy to use and allows you to filter your inventory file on a subset of the hosts you may want to query with your Python scripts.
