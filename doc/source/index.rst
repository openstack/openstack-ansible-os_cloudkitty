============================
OpenStack-Ansible CloudKitty
============================

This Ansible role installs and configures OpenStack CloudKitty.
This role will install the following Upstart services:

.. code-block:: bash

    cloudkitty-api
    cloudkitty-processor

Adding The Service to Your OpenStack-Ansible Deployment
-------------------------------------------------------

To add a new service to your OpenStack-Ansible (OSA) deployment:

* Define ``rating_hosts`` in your ``conf.d`` or
  ``openstack_user_config.yml``. For example:

  .. code-block:: yaml

      rating_hosts:
        infra1:
          ip: 172.20.236.111
        infra2:
          ip: 172.20.236.112
        infra3:
          ip: 172.20.236.113

* Create respective LXC containers (skip this step for metal deployments):

  .. code-block:: console

     openstack-ansible openstack.osa.containers_lxc_create --limit cloudkitty_all,rating_hosts

* Run service deployment playbook:

  .. code-block:: console

     openstack-ansible openstack.osa.cloudkitty

For more information, please refer to the `OpenStack-Ansible project documentation <https://docs.openstack.org/project-deploy-guide/openstack-ansible/latest/>`_.

Always verify that the integration is successful and that the service behaves
correctly before using it in a production environment.


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
