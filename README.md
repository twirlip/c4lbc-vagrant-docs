Installing Evergreen in a Vagrant VM
====================================

A rough guide from code4lib BC, 27 November 2014.

1. Download VirtualBox from https://www.virtualbox.org

2. Download vagrant from http://www.vagrantup.com

3. Create a Debian VM:

<code>
    mkdir vagrant
    cd vagrant/
    vagrant init chef/debian-7.7-i386
    vagrant up
    vagrant ssh
</code>

(We're creating a Debian 7.7 box because it's easiest to install
Evergreen on that type of system.  You can find many other Vagrant boxes
on http://vagrantcloud.com .)

4. Grab the Evergreen install scripts for Debian:

<code>
    git clone git://git.evergreen-ils.org/working/random.git
    cd random
    git checkout collab/phasefx/wheezy_installer
</code>

5. Run the install script.

