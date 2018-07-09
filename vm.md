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

   __NOTE__: Windows 7 users, please be sure that you have Powershell Version 3 installed. Contact [LTS](mailto:librarytechhelp@wustl.edu) if you need assistance.

   Test vagrant installation:

        $ vagrant --version

3. Make a workspace/directory:

        $ cd workspace
        $ mkdir vagrant_tutorial
        $ cd vagrant_tutorial
        $ vagrant init

    __NOTE__: This will create a Vagrantfile in the directory you are in.


4. Edit Vagrantfile to install a Centos/7 box.

   Comment out line with "#":

        # config.vm.box = 'base'

   Add lines:

        config.vm.box = "centos/7"
        config.vm.hostname = "vagrant-tutorial.test"

   Save the Vagrantfile with edit


5. Run "vagrant up" from the director you created:

        $ pwd
        vagrant_tutorial
        $ vagrant up

   __NOTE__: This will create a centos/7 virtual machine. Wait for "vagrant up" to finish.

6. You can ssh into the VM you created in step 5 (these commands are run from your host computer, these commands let you enter the virtual machine you created

        $ pwd
        vagrant_tutorial
        $ vagrant ssh

7. Explore the CentOS/7 environment that you created (these commands are run from within the virtual machine)

        $ hostname
        $ ls -la
        $ ip address
        $ exit (disconnects you from VM)


8. Halt the virtual machine OR destroy the virtual machine you created (commands from your host machine).

   __Halt VM__: this will only stop the VM running on your local machine. If you were to vagrant up, it would turn the machine on (but since you already downloaded box and configured it, it will take much less time)

        $ vagrant halt

   __Destroy VM__: this will completely remove the VM from your local computer and any configurations you performed

        $ vagrant destroy
        (and then type Y at prompt)

   __NOTE__: This will require you to run a "vagrant up" and download the box again from Vagrant (same as initial vagrant up above)



---
---

### Vagrant Components

---

#### Vagrantfile

This is the file that gets generated when you do "vagrant init" from your project directory. This is the file that is used to configure your virtual machine. Configuration options are in the next section.

__NOTE__: Vagrant files do not have a file extension.

#### Boxes

Vagrant uses boxes as the "package" or file format of the virtual environment. A box can be used by anyone on any Vagrant supported platform and it will bring up the same working environment for all to use.
There are publicly available boxes via the [Vagrant Box Catalog](https://app.vagrantup.com/boxes/search)

You can also create your own boxes to share with others. This allows you to install/configure/create base virtual machines that you can share with others, including any software or applications you want to install.

#### Commands (CLI)

In order to use Vagrant, you must become comfortable with the command-line interface. All Vagrant commands begin with "vagrant."

__NOTE__: Most commands should be run from the project directory where the Vagrant file is located (i.e. your working project directory)

    $ vagrant OR vagrant -h

*__This will provide help documentation__*

    $ vagrant init

*__This will initialize a project with a Vagrantfile.__* Generally you will only use this at the beginning of a project.

    $ vagrant up

*__This command will create and configure the virtual machine based on the Vagrantfile.__*

    $ vagrant halt

*__This command shuts down the machine that Vagrant is managing.__* For linux virtual machines, this command will use the shutdown command.
You can also use the --force flag to turn the virtual machine off.

    $ vagrant suspend

*__This command suspends rather than turning off or destroying a virtual machine._* A suspend saves the state of the virtual machine so you can resume later.
It does not do a full boot when resume, but restarts from

    $ vagrant resume

*__This command will restart a machine that is turned off via a vagrant suspend command.__* You can then start from where you left off with the virtual machine.

    $ vagrant reload

*__This command is the equivalent of a vagrant halt and then a vagrant up__* Use this command if you make any changes to the Vagrantfile for them to take effect.

    $ vagrant destroy

*__This command will stop the running virtual machine and destroys all resources that were created or configured in the virtual machine.__* You can also use the --force flag here.

    $ vagrant status

*__This tells you the state of the active Vagrant environments you are working on (via the current working project directory)__*

    $ vagrant global-status

*__This tells you the state of all Vagrant environments on your host machine/computer, not just the working directory instance(s)__*



### Configuration Options

---

These settings will help you configure your virtual machine.

__config.vm.box__ : This tells Vagrant which type of machine to bring up (Ex. ubuntu, centos, windows, etc.)

    config.vm.box = "centos/7"

__config.vm.box_version__ : The version of the box to use. This defaults to the latest version of the box available. Example: For CentOS version, you could define an earlier version that current (1804.02 as of 7/9/18).

    config.vm.box_version = "1802.01"

__config.vm.communicator__ : The way you connect to the virtual machine. This defaults to "ssh," but for Windows you can set this to "winrm."

__config.vm.guest__ : The guest OS that will be running on the virtual machine. This defaults to ":linux," but can be set to ":windows."

__config.vm.hostname__ : You can set the hostname of the Guest OS. This defaults to nil and Vagrant will not manage the hostname. If set, Vagrant will set hostname on boot.

    config.vm.hostname = "lib-dev.test"


__config.vm.network__ : This helps you configure the network for the on the virutal machine you are creating

    config.vm.network "forwarded_port", guest: 80, host: 8080

This will allow you to access a website configured on the virtual machine on port 80 from your host machine (running the VM) from localhost:8080

    config.vm.network "private_network", ip: "192.168.33.10"

This sets a static ip within your private network; it is up to you to make sure that these do not conflict with other running virtual machines (i.e. make sure they are unique)

you can also set this to type: "dhcp" to have it auto-assign a private network IP and then ssh into the machine to determine the address (via ip addr or ifconfig).

    config.vm.network "public_network"

Just like with private networks, you can set this via dhcp (which is the default), or you can set this to a static IP via the "ip:" flag as seen above.

__NOTE__: Vagrant will prompt you to decide which network interface to use when declaring a public_network. You can also set this via the "bridge:" flag, but it must be an exact match to the name of the interface.

    Example: config.vm.network "public_network"

Generally, for Vagrant machines, you will use private_network to mimic or mirror how the network interactions will work in a production environment.

__config.vm.provision__ : This tells Vagrant how to provision or configure the Guest OS. For our purposes, we will be using Ansible, but you can use chef, puppet, or shell provisioning.

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
    end

__config.vm.synced_folder__ : This allows folders on your host machine to be available on your guest machine. By default, Vagrant will sync your project directory (i.e. where the Vagrantfile is located) to the guest machine.



---
---

### Resources

---

[Getting Started with Vagrant](https://www.vagrantup.com/intro/getting-started/index.html)

[Samvera Vagrant](https://github.com/samvera-labs/samvera-vagrant)
