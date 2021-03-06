# Ansible Role: HAProxy

[![Build Status](https://travis-ci.org/emmetog/ansible-role-haproxy.svg?branch=master)](https://travis-ci.org/emmetog/ansible-role-haproxy)

Installs HAProxy on RedHat/CentOS and Debian/Ubuntu Linux servers.

**Note**: This role _officially_ supports HAProxy versions 1.4 or 1.5. Future versions may require some rework.

**Note**: This role is based on the [geerlingguy/ansible-role-haproxy](https://github.com/geerlingguy/ansible-role-haproxy) role,
with a few adaptations to make the configuration more flexible.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    haproxy_sockets:
        - "/var/lib/haproxy/stats level admin"

The sockets through which HAProxy can communicate (for admin purposes or statistics). To disable/remove this directive, set `haproxy_sockets: []` (an empty array).

    haproxy_chroot: /var/lib/haproxy

The jail directory where chroot() will be performed before dropping privileges. To disable/remove this directive, set `haproxy_chroot: ''` (an empty string). Only change this if you know what you're doing!

    haproxy_user: haproxy
    haproxy_group: haproxy

The user and group under which HAProxy should run. Only change this if you know what you're doing!

    haproxy_log_destination: /dev/log

The destination to log to, you won't normally need to change this from it's default value.

    haproxy_backend_timeout: 50000

The timeout in ms for backend servers to fully respond. It is recommended by HAProxy that the client timeout
be the same, for this reason this value will be used for both server and client timeouts.

    haproxy_defaults: []
      # - "option forwardfor"
      
Any configuration that should be added to the "defaults" section.

    haproxy_frontend_servers: []
      # - name: http-in
      #   bind:
      #     - "*:80"
      #   extra_configs:
      #     - "acl host_bacon hdr(host) -i ilovebacon.com"
      #     - "use_backend bacon_cluster if host_bacon"
    
HAProxy frontend configuration directives.

    haproxy_backend_servers: []
      # - name: bacon_cluster
      #   balance_method: leastconn
      #   servers:
      #     - name: node1
      #       address: 10.0.0.1:8080
      #   extra_configs:
      #     - "option httpclose"
      #     - "cookie JSESSIONID prefix"

HAProxy backend configuration directives.

    haproxy_global_vars:
      - 'ssl-default-bind-ciphers ABCD+KLMJ:...'
      - 'ssl-default-bind-options no-sslv3'

A list of extra global variables to add to the global configuration section inside `haproxy.cfg`.

    haproxy_restarted_status: restarted
    
The status the haproxy service should be in when this role changes some configuration.

## Dependencies

None.

## Example Playbook

    - hosts: balancer
      sudo: yes
      roles:
        - { role: geerlingguy.haproxy }

## License

MIT / BSD

## Author Information

This role was origianlly created in 2015 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/),
this fork is maintained by Emmet O'Grady.
