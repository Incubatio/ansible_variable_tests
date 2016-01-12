=====================
Ansible variable test
=====================

This repo attempt to demonstrate how ansible uses variable.


Test scenario:
~~~~~~~~~~~~~~

we have two fictive services, apiserver and gameserver.
Both service consume the same database technology: fictive_db.
For the Launch of a new game, we want to deploy at first those services on a single server.
While developping the deploy script, we want to keep in mind that one day the game might get success, and require several server.

For those reason, we will use one instance of fictive_db per service and run it on a specific port (aka db_port).


Test settings:
~~~~~~~~~~~~~~

For each test we will use two host files:

- hosts-host_variable
- hosts-group_variable

apiserver will have it's fictive_db instance running on db_port=1111

gameserver will have it's fictive_db instance running on db_port=2222

webserver is a group of [apiserver and gameserver]

Each test is simply using ``debug: {{ inventory_hostname }} has value {{ db_port }}`` in a role and in a task.


Test1
------

**Description:** define 2 group pointing to the same ip address.


Results for ``ansible-playbook -i test1/hosts-host_variable.ini site.yml``::

  (webservers)  "msg": "[ROLE] 192.168.56.2 has value 2222"
  (webservers)  "msg": "[TASK] 192.168.56.2 has value 2222"
  (apiservers)  "msg": "[ROLE] 192.168.56.2 has value 2222"
  (apiservers)  "msg": "[TASK] 192.168.56.2 has value 2222"
  (gameservers) "msg": "[ROLE] 192.168.56.2 has value 2222"
  (gameservers) "msg": "[TASK] 192.168.56.2 has value 2222"


Results for ``ansible-playbook -i test1/hosts-group_variable.ini site.yml``::

  (webservers)  "msg": "[ROLE] 192.168.56.2 has value 2222"
  (webservers)  "msg": "[TASK] 192.168.56.2 has value 2222"
  (apiservers)  "msg": "[ROLE] 192.168.56.2 has value 2222"
  (apiservers)  "msg": "[TASK] 192.168.56.2 has value 2222"
  (gameservers) "msg": "[ROLE] 192.168.56.2 has value 2222"
  (gameservers) "msg": "[TASK] 192.168.56.2 has value 2222"




Test2
------

**Description:** define 2 group pointing to two different domain, each domain pointing to the same ip

Results for ``ansible-playbook -i test2/hosts-host_variable.ini site.yml``::

  (webservers)  "msg": "[ROLE] a1.dev has value 1111"
  (webservers)  "msg": "[TASK] a1.dev has value 1111"
  (webservers)  "msg": "[ROLE] a2.dev has value 2222"
  (webservers)  "msg": "[TASK] a2.dev has value 2222"
  (apiservers)  "msg": "[ROLE] a1.dev has value 1111"
  (apiservers)  "msg": "[TASK] a1.dev has value 1111"
  (gameservers) "msg": "[ROLE] a2.dev has value 2222"
  (gameservers) "msg": "[TASK] a2.dev has value 2222"

Results for ``ansible-playbook -i test2/hosts-group_variable.ini site.yml``::

  (webservers)  "msg": "[ROLE] a1.dev has value 1111"
  (webservers)  "msg": "[TASK] a1.dev has value 1111"
  (webservers)  "msg": "[ROLE] a2.dev has value 2222"
  (webservers)  "msg": "[TASK] a2.dev has value 2222"
  (apiservers)  "msg": "[ROLE] a1.dev has value 1111"
  (apiservers)  "msg": "[TASK] a1.dev has value 1111"
  (gameservers) "msg": "[ROLE] a2.dev has value 2222"
  (gameservers) "msg": "[TASK] a2.dev has value 2222"
