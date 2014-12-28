# Ansible Role: RabbitMQ

This role will install RabbitMQ on the spedified hosts, it's part of the ELAO Ansible stack but can be used stand alone.

## Requirements

- Ansible 1.7.2+

## Dependencies

None.

## Installation

Using ansible galaxy:

```bash
ansible-galaxy install elao.rabbitmq
```
You can add this role as a dependency for other roles by adding the role to the meta/main.yml file of your own role:

```yaml
dependencies:
  - { role: elao.rabbitmq }
```


## Role Handlers

    rabbitmq-server restart  # Restart RabbitMQ service


## Role Variables

### Environnement


|Name|Default|Description|
|----|----|-----------|
`NODE_IP_ADDRESS`|Empty string - meaning bind to all network interfaces.|Change this if you only want to bind to one network interface. To bind to two or more interfaces, use the tcp_listeners key in rabbitmq.config.
`NODE_PORT`|5672|
`NODENAME`|`rabbit@$HOSTNAME`|The node name should be unique per erlang-node-and-machine combination.

See the [**RabbitMQ documentation**](http://www.rabbitmq.com/configure.html#define-environment-variables) for the others environnement variables.

### Configuration example

```
---

elao_rabbitmq_signing_key:  http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
elao_rabbitmq_repository:   deb http://www.rabbitmq.com/debian/ testing main

elao_rabbitmq_user:         rabbitmq
elao_rabbitmq_group:        rabbitmq

# Plugins
elao_rabbitmq_plugins:      []
elao_rabbitmq_new_only:     "no"

# Vhosts and Users
elao_rabbitmq_vhosts:       []
elao_rabbitmq_users:        []

# Configuration
elao_rabbitmq_tcp_listeners:
    -
        address:    127.0.0.1
        port:       5672
    - 
        address:    "{{ ansible_venet0_0.ipv4.address }}"
        port:       5672

elao_rabbitmq_ssl_listeners:
    -
        address:    127.0.0.1
        port:       5671
    - 
        address:    "{{ ansible_venet0_0.ipv4.address }}"
        port:       5671

elao_rabbitmq_handshake_timeout:     10000
elao_rabbitmq_ssl_handshake_timeout: 5000
```

Be carefull a previous declared Vhost which no more defined vhost will be deleted !!

## Example playbook

    - hosts: servers
      roles:
         - { role: elao.rabbitmq }

# Licence

MIT

# Author information

ELAO [**(http://www.elao.com/)**](http://www.elao.com)