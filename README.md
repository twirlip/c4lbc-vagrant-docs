c4lbc-vagrant-docs
==================

1. Download VirtualBox from https://www.virtualbox.org

2. Download vagrant from http://www.vagrantup.com

3. Create a Debian VM:

mkdir vagrant
cd vagrant/
vagrant init chef/debian-7.7-i386
vagrant up
vagrant ssh

(We're creating a Debian 7.7 box because it's easiest to install
Evergreen on that type of system.  You can find many other Vagrant boxes
on http://vagrantcloud.com .)

4. Grab the Evergreen install scripts for Debian:

git clone git://git.evergreen-ils.org/working/random.git
cd random
git checkout collab/phasefx/wheezy_installer

5. Run the install script.

