---
layout: page
title: Linux Command Line
permalink: /linux/
---

## Basics of Linux command line & troubleshooting

Don't fear the prompt:

        $

Often times servers, applications, software, or tools require users to enter commands or perform tasks from a command line prompt (the $ sign above is a default prompt for many Linux systems).

Learning some basic Linux commands can be helpful.

Let's bring up a basic Centos/7 Vagrant machine to work with (use the tutorial content on the [VM Page](https://wulib-wustl-edu.github.io/vm/) if you need help.)

---
---

### Creating a test Vagrant Virtual Machine

1. [Download & Install Virtualbox](https://www.virtualbox.org/wiki/Downloads)
2. [Download & Install Vagrant](https://www.vagrantup.com/downloads.html)

   __NOTE__: Windows 7 users, please be sure that you have Powershell Version 3 installed. Contact [LTS](mailto:librarytechhelp@wustl.edu) if you need assistance.

   Test vagrant installation:

        $ vagrant --version

3. Make a workspace/directory:

        $ mkdir linux_tutorial
        $ cd linux_tutorial
        $ vagrant init

    __NOTE__: This will create a Vagrantfile in the directory you are in.


4. Edit Vagrantfile to install a Centos/7 box.

   Comment out line with "#":

        # config.vm.box = 'base'

   Add lines:

        config.vm.box = "centos/7"
        config.vm.hostname = "linux-tutorial.test"

   Save the Vagrantfile with edit


5. Run "vagrant up" from the directory you created:

        linux_tutorial
        $ vagrant up

   __NOTE__: This will create a centos/7 virtual machine. Wait for "vagrant up" to finish.

6. You can ssh into the VM you created in step 5 (these commands are run from your host computer, these commands let you enter the virtual machine you created)

        $ vagrant ssh

    __NOTE__: run "vagrant ssh" from the linux_tutorial directory


### Where am I?

---

Once you have a Vagrant Virtual Machine up and running, "vagrant ssh" into the vm.

It is really helpful to know where you are on a linux server/filesystem.

Present Working Directory:

    $ pwd






### VI CHEAT SHEET

---

#### FILE OPERATIONS

    * Saving - ":w" (write)
    * Exiting/Closing a file - ":q" (quit)
    * Save and Exit - ":wq" (write, quit)
    * Save and Exit even if you have written something - ":q!"

#### NAVIGATION

    * Jump down a number of lines?
    * Arrow keys, duh
    * H, M, or L (head, middle, last)
    * Page-Down - CTRL-F
    * Page-Up - CTRL-B
    * Jump to the start - 1G
    * Jump to the end - G

#### INSERT & APPEND

    * i - before cursor, a - after cursor
    * I - before beginning of line, A - after end of line

#### DELETING

    * D - delete to end of line
    * dd - delete entire line

#### MOVING TEXT AROUND

    * Highlight/Select text: "v" to start visually selecting, then arrow around
    * Then either "Cut" with "d" (delete) OR
    * "Copy" with "y" (yank) THEN
    * "Paste" with "p" (paste)

#### MISTAKES

    * Undo last edit - "u"
    * Repeat last edit - "."

#### SEARCH AND REPLACE

    * Search forward - "/pattern"
    * Search backward - "/?pattern"
    * Search that forward again - "/"
    * Search that backward again - "?"

#### SETTINGS

    * ESC :   set number OR set nonumber