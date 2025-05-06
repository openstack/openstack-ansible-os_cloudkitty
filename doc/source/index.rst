============================
OpenStack-Ansible CloudKitty
============================

This Ansible role installs and configures OpenStack CloudKitty.
This role will install the following Upstart services:

.. code-block:: bash

    cloudkitty-api
    cloudkitty-processor

Required Variables
==================

.. code-block:: yaml

   external_lb_vip_address: 172.16.24.1
   internal_lb_vip_address: 192.168.0.1
   cloudkitty_galera_address: "{{ internal_lb_vip_address }}"
   cloudkitty_container_mysql_password: "SuperSecretePassword1"
   cloudkitty_service_password: "SuperSecretePassword2"
   cloudkitty_rabbitmq_password: "SuperSecretePassword3"

Example Playbook
================

.. code-block:: yaml

    - name: Install cloudkitty service
      hosts: cloudkitty_all
      user: root
      roles:
        - { role: "os_cloudkitty", tags: [ "os-cloudkitty" ] }
      vars:
        external_lb_vip_address: 172.16.24.1
        internal_lb_vip_address: 192.168.0.1
        cloudkitty_galera_address: "{{ internal_lb_vip_address }}"
        cloudkitty_container_mysql_password: "SuperSecretePassword1"
        cloudkitty_service_password: "SuperSecretePassword2"
        cloudkitty_oslomsg_rpc_password: "SuperSecretePassword3"
        cloudkitty_oslomsg_notify_password: "SuperSecretePassword4"

Dependencies
~~~~~~~~~~~~

This role needs pip >= 7.1 installed on the target host.
