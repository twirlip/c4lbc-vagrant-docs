Installing Evergreen in a Vagrant VM
====================================

A rough guide from code4lib BC, 27 November 2014.


Prerequisites
-------------

Download VirtualBox from https://www.virtualbox.org.

Download Vagrant from http://www.vagrantup.com.

I recommend reading through the [Getting Started][1] guide for Vagrant,
so that you know what you're doing when you follow the instructions
below. :)

[1]: https://docs.vagrantup.com/v2/getting-started/index.html


Setting up a Virtual Machine with Vagrant
-----------------------------------------

Create a working directory for your installation.

<pre><code>mkdir vagrant
cd vagrant/
</code></pre>

Set up a VM.  We'll use Debian because it's easiest to install Evergreen
on that flavor of Linux (you can find many other types of Vagrant boxes
on http://vagrantcloud.com):

<pre><code>vagrant init chef/debian-7.7-i386
</code></pre>

In order to download and run the Evergreen install script, we'll need to
install git on the VM, and the script itself works best when run as a
user with sudo NOPASSWD privileges.  We can handle these steps with
Vagrant's [provisioning][2] feature.  Create a script named `bootstrap.sh`
containing the following:

<pre><code>#!/bin/bash
echo 'vagrant ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/eg-installer
apt-get update
apt-get install -y git-core
</code></pre>

Next, add this line to the main configuration loop in your Vagrantfile:

<code>config.vm.provision :shell, path: "bootstrap.sh"</code>

Now we can actually create our VM:

<code>vagrant up</code>

At this point, the VM should be up and running!

[2]: https://docs.vagrantup.com/v2/getting-started/provisioning.html


Installing Evergreen
--------------------

In your working directory, create a script called `install-eg.sh`
containing the following:

<pre><code>#!/bin/bash

# grab a copy of the Evergreen automated install script for Debian
git clone git://git.evergreen-ils.org/working/random.git
cd random/
git checkout -b wheezy origin/collab/phasefx/wheezy_installer
cd installer/wheezy

# run the script to install Evergreen with sample data
time sudo ./eg_wheezy_installer.sh -y -a -s
</code></pre>

Make this script executable:

<code>chmod +x install-eg.sh</code>

Through the magic of Vagrant [synced folders][3], this script (and
everything else in our working directory) will be accessible from within
our VM.  So let's login to the VM and install Evergreen!

<pre><code>vagrant ssh
cd /vagrant
./install-eg.sh
</code></pre>

The install process will take a while.  Once it's done, open up a web
browser and go to `http://localhost:8080`, and you should see the
default OPAC for your Evergreen install.

[3]: https://docs.vagrantup.com/v2/getting-started/synced_folders.html

