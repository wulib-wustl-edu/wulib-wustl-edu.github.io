---
layout: page
title: Configuration Management
permalink: /config/
---

### Configuration Management

---

#### What is it?

Configuration Management is a process by which computers or servers are setup and managed, as well as maintaining the integrity and changes to those servers over time.

Automation is a key component of configuration management. Using configuration management tools such as Chef, Puppet, or Ansible, scripts can be written to provision servers.
These scripts can be used across many servers so that the exact same configuration applies to all the servers the script is deployed.

Instead of logging into each machine and installing or configuring packages, configuration management tools automate this process.

The goal of configuration management tools is to make servers and systems match the state described in your provisioning scripts.



__Key Features:__

1. __Automate the creation and provisioning of new servers__

    Once you have a working provision script that configures a server, this script can be used to provision a new server exactly like the first one.

2. __Recover from critical events__

    If a server goes down (for any number of reasons), a new server can be setup quickly to replace or fill-in for the server that is down.
    This gives system administrators the ability to troubleshoot the server that is down without causing extensive downtime for users.

3. __No "Unique" Servers__

    If one server is manually configured differently from another for the same purpose, it becomes difficult for systems administrators to troubleshoot or even further configure
    the servers. It is hard to know what is installed on each server because sys admins cannot refer back to provisioning scripts used to configure or manage the server and any software installed.

4. __Duplicate Environments__

    Just like #1, provisioning scripts can be used to setup test, staging, and production environments in the exact same way.
    This helps to reduce the amount of problems caused by the Server/OS during deployments


### Ansible

---

As noted, Ansible is an open-source configuration management tool. It is capable of configuring, deploying, and orchestrating IT systems/servers.

It uses OpenSSH to communicate with servers. There is a control machine (from which ansible commands or playbooks are run) and Node machines (the machines to be configured)*[]:

#### Installation


__Control Machine__:

Ansible can be installed on RedHat, Debian, CentOS, OSX, BSDs. Sorry, no Windows installation possible.

You can use the OS' package management system to install Ansible (yum for RedHat/CentOs, apt-get for Debian, homebrew for OSX, etc.)

You can also use Pip. It installs Python Packages available via PyPi or Python Package Index.

    If you have Python 2.7+ installed, you can install Ansible via Pip:

        $ pip install ansible


__Node Requirements__:

The systems must have SSH communication capabilityes, by default via SFTP. Ansible can use SCP if SFTP is not available. For Windows machines, WinRM is required.




#### Getting Started

The best way to test Ansible is to use Vagrant to create virtual machines.
The virtual machines can be used to test Ansible's provisioning scripts against test environments before you create staging or production environments.

As noted on the [VM page](https://wulib-wustl-edu.github.io/vm/), Vagrant virtual machines can be provisioned using Ansible, Chef, Puppet, or even Shell.

Within a Vagrantfile, you can tell Vagrant to build a server (i.e. virtual machine to create), and then provision that server using an Ansible script (or playbook in the Ansible world).

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
    end


Example of playbook.yml:

__Create a playbook.yml file in same directory as Vagrantfile__

    $ touch playbook.yml

OR

    $ echo "" > playbook.yml


Copy/paste the content below into the playbook.yml file



-------------------------------------------------------

{% raw %}
    ---
    - hosts: default
      become: yes
      tasks:
      - name: install default yum packages
        yum:
          name: "{{ item }}"
          state: present
        loop:
          - epel-release
          - "@Development tools"
          - httpd
          - javas
          - net-tools
          - ntp
      - name: establish services
        service:
          name: "{{ item }}"
          state: started
          enabled: yes
        loop:
          - ntpd
          - sshd
          - httpd
          - firewalld

{% endraw %}

The playbook points at the Vagrant host (which is default by default, har har). Tasks are listed and run against the Vagrant virtual machine.

The tasks listed include installing packages and establishing services (enabling and starting various linux software/applications).

The tasks use Ansible [Modules](https://docs.ansible.com/ansible/latest/user_guide/modules_intro.html) to perform the configuration on the server.

There are many Modules available via the Ansible Module [Index](https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html).

For our example playbook, we use the Yum and Service Modules as we are configuring a CentOS virtual machine.



