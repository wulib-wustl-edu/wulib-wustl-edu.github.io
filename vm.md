---
layout: page
title: Virtual Machines
permalink: /vm/
---

### What's a VM?

Volunteer to add this content? Anybody? I can do it but looking for a collaborator/guinea pig.

### Why Vagrant?

---

Vagrant helps developers and devops engineers automate the building and managing of virtual machine environments into a single workflow.

It allows the team to standardize the work environments that will be used for our applications, systems, etc.

Vagrant VMs can be built with VirtualBox, VMWare, AWS, etc.

The Vagrant machines can be provisioned with such tools as Chef, Puppet, Shell scripts, and for our purposes Ansible!


#### Developers

You do not need to worry about dependencies on your local machine anymore. As long as you have Virtualbox and Vagrant installed, you should need ot have a Vagrantfile
that will create the VM "box" for you.

One developer might have a PC and another might have a Mac; if they share the same Vagrantfile, they can develop in the same environment with all its dependencies baked in.


#### DevOps

With provisioning tools like Ansible, you can test the configuration of Virtual Environments before deploying to staging or production environments.

Also, by testing configuration via Ansible locally, costs are reduced. VMs via AWS, Shared Services, etc. are only built when ready for staging and production.

Testing & Development can occur in a standard Vagrant instance and then the Ansible Playbook used to configure that VM used to configure the staging and production environments.


#### Librarians

If you have VirtualBox and Vagrant installed on your work computer and an LTS member shares a Vagrantfile with you, you can run a development environment to test/look/review applications (like Samvera, etc.)

---
---

### Getting Started

---

1. [Download & Install Virtualbox](https://www.virtualbox.org/wiki/Downloads)
2. [Download & Install Vagrant](https://www.vagrantup.com/downloads.html)

   Test vagrant installation:

        $ vagrant --version

3. Make a workspace/directory:

        $ cd workspace
        $ mkdir vagrant_tutorial
        $ cd vagrant_tutorial
        $ vagrant init

    __NOTE__: This will create a Vagrantfile in the directory you are in.


4. Edit Vagrantfile to install a Centos/7 box.

   Add lines:

        config.vm.box = "centos/7"
        config.vm.hostname = "vagrant-tutorial.test"

   Save the Vagrantfile with edit


5. Run "vagrant up" from the director you created:

        $ pwd
        vagrant_tutorial
        $ vagrant up

   __NOTE__: This will create a centos/7 virtual machine. Wait for "vagrant up" to finish.

6. You can ssh into the VM you created in step 5.

        $ pwd
        vagrant_tutorial
        $ vagrant ssh

7. Explore the Centos7 environment that you created

        $ vagrant ssh
        $ hostname
        $ ls -la
        $ ip address

---
---

### Resources

---

- [Getting Started with Vagrant](https://www.vagrantup.com/intro/getting-started/index.html)
- [Samvera Vagrant](https://github.com/samvera-labs/samvera-vagrant)