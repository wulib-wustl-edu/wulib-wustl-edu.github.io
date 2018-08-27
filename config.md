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

You can use the OS' package management system to install Ansible (yum for RedHat/CentOS, apt-get for Debian, homebrew for OSX, etc.)

You can also use Pip. It installs Python Packages available via PyPi or Python Package Index.

    If you have Python 2.7+ installed, you can install Ansible via Pip:

        $ pip install ansible


__Node Requirements__:

The systems must have SSH communication capabilities, by default via SFTP. Ansible can use SCP if SFTP is not available. For Windows machines, WinRM is required.

---

#### Getting Started

In order to get started, you need to setup a control machine. Remember, the Ansible Control Machine needs to be a *nix based system. Windows machines cannot be a controller machine.

To get started, let's use Vagrant to setup a CentOS 7 machine. Please refer to: [VM page](https://wulib-wustl-edu.github.io/vm/) for instructions on how to setup a Virtual Environment.

__Please make sure VirtualBox and Vagrant are installed.__

1. Use Git to clone the Ansible Tutorial repository on Gitlab

        $ cd workspace
        $ git clone https://gitlab.com/wulibrary/ansible_tutorial.git

    __REVIEW__: Look at Vagrantfile to see what's going on. This creates multiple VMs from one Vagrantfile.

2. Navigate into the ansible_tutorial directory and vagrant up

        $ cd ansible_tutorial
        $ vagrant up


5. Wait until your new Vagrant virtual machines (i.e. your new Ansible Control Machine and 2 nodes) and then SSH into the Control Machine

        $ vagrant ssh control1


    __NOTE__: You can vagrant ssh into different machines based on the names you assigned in Vagrantfile

6. Practice some Linux/Unix commands in your new VM while installing Ansible [Ansible Documentation provides instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-machine)

        $ sudo yum install epel-release -y
        $ sudo yum install ansible -y

7. Check your Ansible installation on your VM

        $ ansible --version
        ansible 2.6.1
          config file = /etc/ansible/ansible.cfg
          configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
          ansible python module location = /usr/lib/python2.7/site-packages/ansible
          executable location = /bin/ansible
          python version = 2.7.5 (default, Apr 11 2018, 07:36:10) [GCC 4.8.5 20150623 (Red Hat 4.8.5-28)]


#### Configuration

Once Ansible is installed on your Control Machine (remember it requires *nix Operating Systems), you can look at configuration files.

If you install Ansible using YUM or DNF (package managers), the installation will create an /etc/ansible/ directory on your control machine.

    $ cd /etc/ansible
    $ ls
    ansible.cfg  hosts  roles

__NOTE__:

If you ever install Ansible using pip (Pip Installs Packages), you will need to create your /etc/ansible/ directory and ansible.cfg file.

Please refer to the default file on [Ansible's Github Page](https://raw.githubusercontent.com/ansible/ansible/devel/examples/ansible.cfg)

You can copy/paste the content from the above link into your ansible.cfg file if installing Ansible from Pip.

For our purposes, the default configuration files will work just fine (and most people leave the .cfg file alone). Please refer to the [docs](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings-locations) for more info.

----

#### SSH is important

As mentioned previously, Ansible uses SSH to communicate and interact with Node machines from control machines.

Let's take some time to practice setting up SSH keys, looking at the sshd_config file, and copying our SSH public keys to our node machines (all the while learning how to use Linux basic commands)

__NOTE__: There are other ways to setup SSH keys, but understanding some of the manual steps (i.e. nuts and bolts) can be helpful. Also, understanding how to work with the Linux environment is important for any SysAdmin


1. Login to the Ansible Control Machine you created previously

        $ vagrant ssh control1

2. Create your SSH keys

        $ ssh-keygen

    __NOTE__: Don't Enter A Passphrase for now. Just Press Enter.

3. Take a look at your newly generated SSH keys.

        $ cd ~/.ssh/
        $ ls
        authorized_keys  id_rsa  id_rsa.pub

    __NOTE__:

    Authorized_Keys verify that users logging are who they say they are (the SSH public key is listed in the authorized key file)

    Known_Hosts is a log of all the machines that the user has connected to using their SSH keys


4. Open new terminal window or command prompt, login to Dev 1 machine, and edit sshd_config


        $ vagrant ssh dev1
        $ sudo su
        $ sudo vi /etc/sshd/sshd_config


    From within sshd_config file:

        /PasswordAuthentication (Press Enter)

    The above command searches for "PasswordAuthentication" in the sshd_config file.

    Change it to yes

5. Restart sshd service on the server on Dev 1 machine

        $ sudo systemctl restart sshd


   __NOTE__: Perform same steps on Dev 2 Machine


6. Copy SSH public key using SSH-Copy-ID from Control1 Machine

        $ ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.33.13
        Enter Password (vagrant is password)

        $ ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.33.14
        Enter Password (vagrant is password)


7. Test SSH connection from Control Machine to Dev 1 or 2

        $ ssh vagrant@192.168.33.13

    No Password Should Be Required


---


#### Inventory

When you install Ansible, along with the ansible.cfg file, there is also a hosts file located in /etc/ansible/ directory.

We use this file to define the inventory of hosts (or nodes) that we want to control from our Ansible Control Machine.

Please take a look at the host_tutorial file. This is a basic hosts file depicting the VM servers that we created.

    [dev1]
    192.168.33.13

    [dev2]
    192.168.33.14

    [control:children]
    dev1
    dev2

---

#### Ad-Hoc Commands

We will review playbooks next, but getting an understanding of running ad-hoc commands is important, especially for Sys-Admins like us.

You might need to perform actions really quickly and do not need to save it for later.

Ad-Hoc Commands can be run on the command against one or many hosts.


__NOTE__: Like Coffee or Training Wheels, you should not rely on Ad-Hoc Commands. Please get used to Playbooks.


Now that we have Ansible setup, we have SSH setup, and we have our Inventory file, lets try some Ad-Hoc Ansible Commands:


1. Print out (i.e. echo) the Terminal being used by all children of the Control Machine (look at the Inventory file)

        $ ansible control -m shell -a 'echo $TERM'
        192.168.33.14 | SUCCESS | rc=0 >>
        xterm-256color

        192.168.33.13 | SUCCESS | rc=0 >>
        xterm-256color

2. Print out (i.e. echo) the Shell being used by all children of the Control Machine (look at the Inventory file)

        $ ansible control -m shell -a 'echo $SHELL'
        192.168.33.14 | SUCCESS | rc=0 >>
        /bin/bash

        192.168.33.13 | SUCCESS | rc=0 >>
        /bin/bash

3. Get the hostname of the Dev 1 box

        $ ansible dev1 -a hostname
        192.168.33.13 | SUCCESS | rc=0 >>
        dev1.test

4. Install Apache (via YUM for CentOS) on both Dev servers

        $ ansible control -m yum -a "name=httpd state=present" --become
        192.168.33.13 | SUCCESS => {
            "changed": true,
            "msg": "",
            "rc": 0,
            "results": [
                "Loaded plugins: fastestmirror\nLoading mirror speeds from cached hostfile\n * base: mirrors.gigenet.com\n * epel: mirror.steadfast.net\n * extras: mirror.tzulo.com\n * updates: mirror.cs.uwp.edu\nResolving Dependencies\n--> Running transaction check\n---> Package httpd.x86_64 0:2.4.6-80.el7.centos.1 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package      Arch          Version                        Repository      Size\n================================================================================\nInstalling:\n httpd        x86_64        2.4.6-80.el7.centos.1          updates        2.7 M\n\nTransaction Summary\n================================================================================\nInstall  1 Package\n\nTotal download size: 2.7 M\nInstalled size: 9.4 M\nDownloading packages:\nRunning transaction check\nRunning transaction test\nTransaction test succeeded\nRunning transaction\n  Installing : httpd-2.4.6-80.el7.centos.1.x86_64                           1/1 \n  Verifying  : httpd-2.4.6-80.el7.centos.1.x86_64                           1/1 \n\nInstalled:\n  httpd.x86_64 0:2.4.6-80.el7.centos.1                                          \n\nComplete!\n"
            ]
        }
        192.168.33.14 | SUCCESS => {
            "changed": true,
            "msg": "",
            "rc": 0,
            "results": [
                "Loaded plugins: fastestmirror\nLoading mirror speeds from cached hostfile\n * base: mirrors.gigenet.com\n * epel: mirror.steadfast.net\n * extras: mirror.tzulo.com\n * updates: mirror.cs.uwp.edu\nResolving Dependencies\n--> Running transaction check\n---> Package httpd.x86_64 0:2.4.6-80.el7.centos.1 will be installed\n--> Processing Dependency: httpd-tools = 2.4.6-80.el7.centos.1 for package: httpd-2.4.6-80.el7.centos.1.x86_64\n--> Processing Dependency: /etc/mime.types for package: httpd-2.4.6-80.el7.centos.1.x86_64\n--> Running transaction check\n---> Package httpd-tools.x86_64 0:2.4.6-80.el7.centos.1 will be installed\n---> Package mailcap.noarch 0:2.1.41-2.el7 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package           Arch         Version                     Repository     Size\n================================================================================\nInstalling:\n httpd             x86_64       2.4.6-80.el7.centos.1       updates       2.7 M\nInstalling for dependencies:\n httpd-tools       x86_64       2.4.6-80.el7.centos.1       updates        90 k\n mailcap           noarch       2.1.41-2.el7                base           31 k\n\nTransaction Summary\n================================================================================\nInstall  1 Package (+2 Dependent packages)\n\nTotal download size: 2.8 M\nInstalled size: 9.6 M\nDownloading packages:\n--------------------------------------------------------------------------------\nTotal                                              2.7 MB/s | 2.8 MB  00:01     \nRunning transaction check\nRunning transaction test\nTransaction test succeeded\nRunning transaction\n  Installing : httpd-tools-2.4.6-80.el7.centos.1.x86_64                     1/3 \n  Installing : mailcap-2.1.41-2.el7.noarch                                  2/3 \n  Installing : httpd-2.4.6-80.el7.centos.1.x86_64                           3/3 \n  Verifying  : mailcap-2.1.41-2.el7.noarch                                  1/3 \n  Verifying  : httpd-tools-2.4.6-80.el7.centos.1.x86_64                     2/3 \n  Verifying  : httpd-2.4.6-80.el7.centos.1.x86_64                           3/3 \n\nInstalled:\n  httpd.x86_64 0:2.4.6-80.el7.centos.1                                          \n\nDependency Installed:\n  httpd-tools.x86_64 0:2.4.6-80.el7.centos.1    mailcap.noarch 0:2.1.41-2.el7   \n\nComplete!\n"
            ]
        }

5. Start Apache Service on Both Dev Servers

        $ ansible control -m service -a "name=httpd state=started" --become



__NOTE__: Modules are the way Ansible connects to the tools and services on servers (i.e. there is a yum module to work with yum commands on the server)

[Module Documentation](https://docs.ansible.com/ansible/2.5/user_guide/modules_intro.html)

---

#### Playbooks

The best way to test Ansible is to use Vagrant to create virtual machines.
The virtual machines can be used to test Ansible's provisioning scripts against test environments before you create staging or production environments.

As noted on the [VM page](https://wulib-wustl-edu.github.io/vm/), Vagrant virtual machines can be provisioned using Ansible, Chef, Puppet, or even Shell.

Within the Vagrantfile from the tutorial repo, you can tell Vagrant to build a server (i.e. virtual machine to create), and then provision that server using an Ansible script (or playbook in the Ansible world).

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
    end


-------------------------------------------------------

{% raw %}
    ---
    - hosts: dev1, dev2
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
          - java
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


---

#### Running an Ansible Playbook


When you want to run Ansible Playbooks against Nodes (Servers/Machines you want to configure), you can run them from the command line.


1. Login to the Control Machine

        $ vagrant ssh control1

2. Move to the /vagrant directory (which was copied over when we setup the VMs)

        $ cd /vagrant
        $ ls
        hosts_tutorial  playbook.yml  README.md  Vagrantfile



3. Check to see which Servers the playbook will run against

        $ ansible-playbook playbook.yml --list-hosts

        playbook: playbook.yml

          play #1 (control): control	TAGS: []
            pattern: [u'control']
            hosts (2):
              192.168.33.14
              192.168.33.13


4. Run the Playbook with a Dry-Run (this will not make any changes)

        $ ansible-playbook playbook.yml --check


5. Run the Playbook (no really this time)

        $ ansible-playbook playbook.yml


---

#### Roles


Roles are packaged variables, tasks, and files that can be used to provision servers with certain configurations.

[Ansible Galaxy](https://galaxy.ansible.com) is a free and open repository where people share Roles they have developed and shared for reuse by the community.

You can install roles on your Control Machine and use them in your playbooks.


1. Login to the Control Machine

        $ vagrant ssh control1


2. Install an Nginx Role from Ansible Galaxy

        $ ansible-galaxy install geerlingguy.nginx


3. Add the Role to your Playbook (and save edits)

        $ sudo vi playbook.yml

        - hosts: control
          become: yes
          <<<< NEW STUFF
          roles:
             - geerlingguy.nginx
          >>>>
          tasks:

        wq! (this saves the file in vi)

4. Run the Playbook

        $ ansible-playbook playbook.yml


__NOTE__: Please take a look at the [Ansible Git Repo](https://gitlab.com/wulibrary/ansible) in our Team's Gitlab instance to see all the Roles that we currently use.



#### Where are Roles installed?

Within the ansible.cfg file located at /etc/ansible/ansible.cfg, there is a Roles Path configuration. By default, roles are installed to /etc/ansible/roles and ~/.ansible/roles. Whichever path is the first writable path is where the roles will be installed.

Take a look on the control machine. Where did the nginx role get installed?

It should be in the ~/.ansible/roles directory for the vagrant user (unless you ran with sudo privileges).

You can also specify where roles are installed by add the --roles-path flag to your ansible-galaxy install command.

Example:

    $ ansible-galaxy install --roles-path <path to where you want to install role> <name of role to install>

    $ cd /vagrant
    $ mkdir roles
    $ ansible-galaxy install --roles-path ./roles/ geerlingguy.nginx




---
---

#### Variables

We have already seen variables used in the playbook above.


__YAML__: Ansible uses the [YAML](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html). Playbooks, Roles, Variables, etc. are written in YAML Syntax.

It might be useful to read about Python Lists & Dictionaries, as Ansible is written in Python and uses these [data structures](https://docs.python.org/3.6/tutorial/datastructures.html).

Basics of Variable Names:

1. Variables should be numbers, letters, and underscores. They should always start with a letter

        Good variable

        http_port: 80

        Bad varibles:

        80_port (starts with a number)
        http-port (no underscore)


2. There are also reserved words that have special meaning in Python and Ansible. Take a look at the list [here](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#what-makes-a-valid-variable-name)



__Review of Variables used thus far:__



1. Variables in Inventory File:

        [dev1]
        192.168.33.13

        [dev2]
        192.168.33.14

        [control:children]
        dev1
        dev2
__NOTE__: The Inventory File is written using INI format, BUT it can be written in YAML as well.

2. Variables in Playbooks:

   The \{\{ item \}\} loops that we see in our current playbook are a type of Variable.

   Jinja2: This is a templating system used in many programming languages and web frameworks today. It is used in the Python world in Flask, Django, and obviously in Ansible.

   We define variables and then use the Jinja templating to utlize the variables.

    Basic example:

    Variable:

    http_port: 80

    Jinja template:

    The server's http port is \{\{ http_port \}\}

    Ansible Task Example:

            ---
            - name: configure firewalld ports
              firewalld:
                port: "{{ http_port }}"
                permanent: true
                state: enabled



    Variables can also be placed directly into a playbook:

            ---
            - hosts: dev1,dev2
              vars:
                http_port: 80

3. Variables in Roles:


    Let's look at the Nginx Role that we used from Ansible Galaxy: https://galaxy.ansible.com/geerlingguy/nginx

    Each role in Ansible Galaxy shows you how to install the role on your computer (you can copy/paste the ansible-galaxy install command).

    It shows that OS Platforms supported, the versions of the Role (the developer has versions of the role that he has deployed)

    It also shows the minimum version of Ansible allowed (this is important as it might require you to upgrade Ansible on your control machine, etc.)

    There are also links to the Github repo where the code is [stored](https://github.com/geerlingguy/ansible-role-nginx).

    As noted in the README in the Nginx role, the Variables are in the defaults/main.yml.


    You can overwrite the variables in the defaults/main.yml file to suit your needs when installing the Nginx role.


    __Variable Precedence__:

    What happens if you have variables you define in your playbook, inventory, and roles? Ansible uses Variable Precedence (kinda like Orders of Operation in Math) to determine which varaible to use.

    Here is the order of precedence from least to greatest (the last listed variables winning prioritization): [from docs](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)


        - role defaults
        - inventory file or script group vars
        - inventory group_vars/all
        - playbook group_vars/all
        - inventory group_vars/*
        - playbook group_vars/*
        - inventory file or script host vars
        - inventory host_vars/*
        - playbook host_vars/*
        - host facts / cached set_facts
        - play vars
        - play vars_prompt
        - play vars_files
        - role vars (defined in role/vars/main.yml)
        - block vars (only for tasks in block)
        - task vars (only for the task)
        - include_vars
        - set_facts / registered vars
        - role (and include_role) params
        - include params
        - extra vars (always win precedence)

---
---

#### Exercises, Exploration, and Experimentation:

---

Sometimes the best way to learn is to experiment. Using the control and dev machines you created, try the exercises below.

1. Find a role on Ansible-Galaxy for MySQL (compatible with Centos/RHEL 7) and install to the roles directory you created above on the control machine. Example: https://galaxy.ansible.com/geerlingguy/mysql
2. Add the mysql role to your playbook on the control machine
3. Run the playbook again
4. Try changing the variables for the role so that the defaults are not used
5. How can you install nginx if apache is installed on a machine already?
6. Find other roles that you might need in your daily work on Ansible Galaxy and test installing them
7. Look through documentation and online resources to find out how to get information or "setup" information about a node machine (i.e. host you are trying to configure using ansible)

