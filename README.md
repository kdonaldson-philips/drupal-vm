![Drupal VM Logo](https://raw.githubusercontent.com/geerlingguy/drupal-vm/master/docs/images/drupal-vm-logo.png)

[![Build Status](https://travis-ci.org/geerlingguy/drupal-vm.svg?branch=master)](https://travis-ci.org/geerlingguy/drupal-vm) [![Documentation Status](https://readthedocs.org/projects/drupal-vm/badge/?version=latest)](http://docs.drupalvm.com) [![Packagist](https://img.shields.io/packagist/v/geerlingguy/drupal-vm.svg)](https://packagist.org/packages/geerlingguy/drupal-vm)

[Drupal VM](https://www.drupalvm.com/) is A VM for local Drupal development, built with Vagrant + Ansible.  This repo includes a config.yml file that prepares the Drupal CM for the Philips Client Experience Portal.

This project aims to make spinning up a simple local Drupal test/development environment incredibly quick and easy.

It will install the following on an Ubuntu 16.04 (by default) linux VM:

  - Apache 2.4.x
  - PHP 7.0.x
  - MySQL 5.7.x
  - Drush
  - Drupal 7.x
  - Optional:
    - Drupal Console
    - Varnish 4.x (configurable)
    - Apache Solr 4.10.x (configurable)
    - Elasticsearch
    - Node.js 0.12 (configurable)
    - Selenium, for testing your sites via Behat
    - Ruby
    - Memcached
    - Redis
    - SQLite
    - XHProf, for profiling your code
    - Blackfire, for profiling your code
    - XDebug, for debugging your code
    - Adminer, for accessing databases directly
    - Pimp my Log, for easy viewing of log files
    - MailHog, for catching and debugging email

It should take 5-10 minutes to build or rebuild the VM from scratch on a decent broadband connection.

## Quick Start Guide

This Quick Start Guide will help you quickly build the Client Experience Portal development environment.

### 1 - Install Vagrant and VirtualBox

Download and install [Vagrant](https://www.vagrantup.com/downloads.html) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

Notes:

  - **NFS on Linux**: *If NFS is not already installed on your host, you will need to install it to use the default NFS synced folder configuration. See guides for [Debian/Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-14-04), [Arch](https://wiki.archlinux.org/index.php/NFS#Installation), and [RHEL/CentOS](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-centos-6).*
  - **Versions**: *Make sure you're running the latest releases of Vagrant, and VirtualBox of February 2016, Drupal VM recommends: Vagrant 1.8.6, VirtualBox 5.1.8+, and Ansible 2.2.x*

### 2 - Build the Virtual Machine

  1. Download the Philips CEP and put it wherever you want.
  2. Download this project and put it wherever you want.
  2. Replace the \<\<LOCATION OF THE DEVELOPERPORTAL\>\> placeholder in 'config.yml` with the path to the CEP repo on your machine
  3. Open Terminal, `cd` to this directory (containing the `Vagrantfile` and this README file).
  4. Install the vagrant `hostsupdater` plugin (`vagrant plugin install vagrant-hostsupdater`), so that Vagrant automatically configure your hosts file. All hosts defined in `apache_vhosts` or `nginx_hosts` will be automatically managed. `vagrant-hostmanager` is also supported.
  5. Install the vagrant `auto_network` plugin (`vagrant plugin install vagrant-auto_network`), so that Vagrant can help with IP address management
  6. Type in `vagrant up`, and let Vagrant do its magic.

Note:
  - On Windows machines, it maybe necessary to enable VT-x or AMD-V in your BIOS
  - Due to a bug in Windows Ruby, it is necessary to reset the Vagrant home:
  -- setx VAGRANT_HOME c:\vagrant
  - If there are any errors during the course of running `vagrant up`, and it drops you back to your command prompt, just run `vagrant provision` to continue building the VM from where you left off. If there are still errors after doing this a few times, send and email to Keith Donaldson.

### 3 - Import the database

  1. Download the latest version of the database from the CEP dev environment.
  2. Go to adminer.pcftest.dev and login
  3. Click 'Import'
  4. Under File upload, click 'Choose Files'
  5. Select the CEP dev database file 
  
  - username: devportal-user
  - password: devportal-pass
  - database: devportal
  
### 4 - Install Apigee Dependencies
  1. Open Terminal, `cd` to this directory (containing the `Vagrantfile` and this README file).
  2. Type in `vagrant ssh` to SSH into a running Vagrant machine and give you access to a shell.
  3. Type 'cd /var/www/pcftest/profiles/apigee/libraries/mgmt-api-php-sdk' in the shell.
  4. Type 'composer install --no-dev' in the shell to install the Apigee dependencies


### 4 - Access CEP
  1. In your browser go to pcftest.dev
  2. Any existing logins on the dev environment will be available here for use

  
## Using Drupal VM

Drupal VM is built to integrate with every developer's workflow. Many guides for using Drupal VM for common development tasks are available on the [Drupal VM documentation site](http://docs.drupalvm.com).

## System Requirements

Drupal VM runs on almost any modern computer that can run VirtualBox and Vagrant, however for the best out-of-the-box experience, it's recommended you have a computer with at least:

  - Intel Core processor with VT-x enabled
  - At least 4 GB RAM (higher is better)
  - An SSD (for greater speed with synced folders)

## Other Notes

  - To shut down the virtual machine, enter `vagrant halt` in the Terminal in the same folder that has the `Vagrantfile`. To destroy it completely (if you want to save a little disk space, or want to rebuild it from scratch with `vagrant up` again), type in `vagrant destroy`.
  - To log into the virtual machine, enter `vagrant ssh`. You can also get the machine's SSH connection details with `vagrant ssh-config`.
  - When you rebuild the VM (e.g. `vagrant destroy` and then another `vagrant up`), make sure you clear out the contents of the `drupal` folder on your host machine, or Drupal will return some errors when the VM is rebuilt (it won't reinstall Drupal cleanly).
  - You can change the installed version of Drupal or drush, or any other configuration options, by editing the variables within `config.yml`.
  - Find out more about local development with Vagrant + VirtualBox + Ansible in this presentation: [Local Development Environments - Vagrant, VirtualBox and Ansible](http://www.slideshare.net/geerlingguy/local-development-on-virtual-machines-vagrant-virtualbox-and-ansible).
  - Learn about how Ansible can accelerate your ability to innovate and manage your infrastructure by reading [Ansible for DevOps](http://www.ansiblefordevops.com/).

## License

This project is licensed under the MIT open source license.

## About the Author

[Jeff Geerling](http://www.jeffgeerling.com/) created Drupal VM in 2014 for a more efficient Drupal site and core/contrib development workflow. This project is featured as an example in [Ansible for DevOps](http://www.ansiblefordevops.com/).
