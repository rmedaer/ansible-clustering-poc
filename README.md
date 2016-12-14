# Hosts clustering in Ansible

This repository contains a small example of hosts clustering in Ansible.
The goal is to define a methodology to implement use cases such as
HA clustering or load sharing.

In our `inventory` we are using a group (here `webservers`) to define
our clusters (see `webservers: children`).
Each cluster is another (sub)group named `<group_name>/<clustername>`.
Each cluster contains its own hosts (e.g. `webservers/paris`).

The role `clustering` add 2 facts on the fly:

  - **cluster**: the name of first cluster found in given group for targeted host.
  - **cluster\_hosts**: the list of alternative hosts in found cluster.

## Run

Let's run Ansible with example inventory and site:

```
ansible-playbook -i inventory site.yml
```

You should have the following output:

```
PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
(...)

TASK [clustering : Setting up clusters] ****************************************
(...)

TASK [clustering : Building host redundancy lists] *****************************
(...)

TASK [debug] *******************************************************************
ok: [madrid-1] => {
    "msg": "cluster: webservers/madrid; alt nodes: "
}
ok: [paris-1] => {
    "msg": "cluster: webservers/paris; alt nodes: paris-2"
}
ok: [paris-2] => {
    "msg": "cluster: webservers/paris; alt nodes: paris-1"
}
ok: [brussels-1] => {
    "msg": "cluster: webservers/brussels; alt nodes: brussels-2, brussels-3"
}
ok: [brussels-2] => {
    "msg": "cluster: webservers/brussels; alt nodes: brussels-1, brussels-3"
}
ok: [brussels-3] => {
    "msg": "cluster: webservers/brussels; alt nodes: brussels-1, brussels-2"
}

PLAY RECAP *********************************************************************
(...)
```
## Contribute

Would like to contribute, share or comment ? Feel free challenge it !

## License

Please consider:

```
"THE BEER-WARE LICENSE" (Revision 42):
Schijfke wrote this file.  As long as you retain this notice you
can do whatever you want with this stuff. If we meet some day, and you think
this stuff is worth it, you can buy me a beer in return.

Raphael Medaer (Schijfke) <rme@escaux.com>
```
