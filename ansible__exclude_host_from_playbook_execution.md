# Ansible – exclude host from playbook execution

By using –limit argument with ansible-playbook command we can exclude a host from playbook execution.
If hostname starts with “!” it will excluded from host execution.

Lets say if we want to exclude host1 and host2 from ansible-playbook execution use following command:
```
$ ansible-playbook --limit '!hoost1:!host2' yourPlaybook.yml
```
To exclude only host1 from execution use following command:
```
$ ansible-playbook --limit '!hoost1' yourPlaybook.yml
```
To execute only in host1 and host2 from execution use following command:
```
$ ansible-playbook --limit 'hoost1:host2' yourPlaybook.yml
```
To execute only in host1 use following command:
```
$ ansible-playbook --limit 'hoost1' yourPlaybook.yml
```
To exclude host1 and host2 from execution and allow execution only in host3:
```
$ ansible-playbook --limit '!hoost1:!host2:host3' yourPlaybook.yml
```