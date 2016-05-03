# Ansible demo utilizing vagrant

This is a small setup to bring you an ansible playground as fast as possible.
We will have some virtualbox debian VMs to operate on, so you are good do whatever you want with ansible in a save environment.

## Prerequisites

You will need virtualbox, vagrant and ansbile installed and about 1,2GB of free RAM.

## Bring it on

To bring the VMs up run:

	vagrant up

Now I recommend to create snapshot so we are able to get back to clean VMs anytime.

	vagrant plugin install vagrant-vbox-snapshot
	vagrant snapshot take vm1 fresh
	vagrant snapshot take vm2 fresh
	vagrant snapshot take vm3 fresh

With these snapshot you can always get fresh VMs with:

	 vagrant snapshot back vm1
	 vagrant snapshot back vm2
	 vagrant snapshot back vm3

Note: To shut down the VMs run:

	vagrant halt

To verify our setup, let's fire our first ansible command:

	ansible all --inventory-file=hosts --module-name command --args "uptime"

You should get something like this:

	192.168.23.10 | success | rc=0 >>
	 10:12:23 up 19 min,  1 user,  load average: 0.00, 0.01, 0.02
	
	192.168.23.30 | success | rc=0 >>
	 10:12:24 up 17 min,  1 user,  load average: 0.00, 0.01, 0.01
	
	192.168.23.20 | success | rc=0 >>
	 10:12:23 up 18 min,  1 user,  load average: 0.00, 0.01, 0.01

So we have three VMs with the IPs 192.168.23.10|20|30

They are defined in the hosts file which ansible will call an inventory.
Have a look at it.

As you can see there are three hosts with the ssh username and ssh key configure.
Ansible connects to remote system via ssh per default.

Also you will have noticed the [firstTwo] and [lastTwo] entries.

These are groups of the above VMs while their settings only have to be defined once.

Let's try these groups:

        ansible firstTwo --inventory-file=hosts --module-name command --args "uptime"

        ansible lastTwo --inventory-file=hosts --module-name command --args "uptime"

Neat, I like ansible so far, do you?

## Playbooks

Instead of running commands via the "command"-module and passing the command via --args, ansible can remember common tasks for you in so called playbooks.

Let's write a simple one:

	---
	- name: "install java"
	  hosts: firstTwo
	  sudo: true
	  tasks:
	  - name: "apt-get update"
	    command: "apt-get update"
	  - name: "install openJDK"
	    apt: name=openjdk-7-jre state=present

and run it with the ansible-playbook command:

	ansible-playbook -i hosts installJava.yml

Note that we run the above playbook on the firstTwo-group only.
Now let's check the result:

	ansible all -i hosts -m command --args "which java"

This is what I get:

	192.168.23.30 | FAILED | rc=1 >>
	
	192.168.23.20 | success | rc=0 >>
	/usr/bin/java
	
	192.168.23.10 | success | rc=0 >>
	/usr/bin/java

So VM1 and VM2 have java installed now and VM3 is untouched. But how exactly did this happen? Let's have a closer look.
We used two modules in the playbook: ```command``` and ```apt```. We knew ```command``` already as we used it as an ad-hoc ansible call.
```apt``` is new and can be used to install, remove, ... packages on debian-based systems.

There is a bunch of more modules e.g. ```yum``` to install packages on CentOS / RHEL systems.
Go and check out all (ansible modles)[].

Of course there is more ansible can do for you but I hope this got you started well.
Check out the (ansible docs)[https://docs.ansible.com/], reset your VMs (```vagrant snapshot back vm1; vagrant snapshot back vm2```) and write your own playbooks.


