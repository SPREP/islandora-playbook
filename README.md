# Installation on bare metal machine

Also works for VM

# Centos 7

```
You need to run below code on host machine and then deploy to another machine. todo: Test if you can deploy to self.

# Download and install vagrant

# Install packages
$ sudo yum install git
$ sudo yum install openssl

# Install Ansible
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py --user

# Then pin down Ansible to 2.8.7
$ pip install --user -Iv ansible==2.8.7

# install vagrant plugin
$ vagrant plugin install vagrant-vbguest

# edit .bashrc and append below
$ export ISLANDORA_DISTRO="centos/7"

# clone repo
$ git clone https://github.com/Islandora-Devops/islandora-playbook
$ cd islandora-playbook
# create deployment profile
$ cp -r inventory/vagrant inventory/example

# update hosts file inventory/example/hosts
default ansible_host=example.org ansible_ssh_host=example.org ansible_ssh_user=root ansible_ssh_private_key_file='/home/username/.ssh/id_rsa'

# get ansible packages and deploy to server
$ ansible-galaxy install -r requirements.yml
$ ansible-playbook -i inventory/production playbook.yml -e "islandora_distro=centos/7"

```

# Islandora Playbook
[![LICENSE](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](./LICENSE)

## Introduction

This is an Ansible playbook for Islandora 8. It also has a vagrant file to bring up a release or development virtual machine for Islandora 8.

This virtual machine **should not** be used in production **yet**.

## Variables

### Base box

By default, Vagrant creates a complete Islandora 8 1.1.0 instance. This version is the current offical release of Islandora.

If you want to build a VM that pulls in the latest Islandora code (suitable for a development environment, for example), before running `vagrant up`, open `Vagrantfile` and change the `$vagrantBox` variable to either 'ubuntu/bionic64' or 'centos/7':

```
# Available boxes are 'islandora/8', ubuntu/bionic64' and 'centos/7'
# Use 'ubuntu/bionic64' or 'centos/7' to build a dev environment from scratch.
# Use 'islandora/8' if you just want to download a ready to run VM.
$vagrantBox = ENV.fetch("ISLANDORA_DISTRO", "islandora/8")
```


### System Resources

By default the virtual machine that is built uses 4GB of RAM. Your host machine will need to be able to support the additional memory use. You can override the CPU and RAM allocation by creating `ISLANDORA_VAGRANT_CPUS` and `ISLANDORA_VAGRANT_MEMORY` environment variables and setting the values. For example, on an Ubuntu host you could add to `~/.bashrc`:

```bash
export ISLANDORA_VAGRANT_CPUS=4
export ISLANDORA_VAGRANT_MEMORY=5040
```

### Using CENTOS

Ubuntu 18.04 is the default Linux distribution used by islandora-playbook. If you want to use CENTOS 7 instead, set the `ISLANDORA_DISTRO` environment variable to `centos/7`. The easiest way to do this is to export the environment variable into your shell before running Vagrant commands. Otherwise you will have to provide the variable for every Vagrant command you issue.

```bash
ISLANDORA_DISTRO="centos/7" vagrant up
ISLANDORA_DISTRO="centos/7" vagrant ssh
```

## Use

Detailed installation and usage instructions can be found on the [official installation documentation for Islandora 8](https://islandora.github.io/documentation/installation/).

## Connect

You can connect to the machine via the browser at [http://localhost:8000](http://localhost:8000).

### Drupal

The default Drupal login details are:

  * username: admin
  * password: islandora

### MySQL

  * username: drupal8
  * password: islandora

### Fedora5

The Fedora 5 REST API can be accessed at [http://localhost:8080/fcrepo/rest](http://localhost:8080/fcrepo/rest).

Authentication is done via [Syn](https://github.com/Islandora-CLAW/Syn) using [JWT](https://jwt.io) tokens.

### Solr

You can access the Solr administration UI at http://localhost:8983/solr/

### SSH

You can connect to the machine via ssh:

  * `vagrant ssh`

### ActiveMQ

The default ActiveMQ login details are:

  * username: admin
  * password: admin

You can access the ActiveMQ administrative interface at: http://localhost:8161/admin

### Cantaloupe

You can access the Cantaloupe admin interface at: http://localhost:8080/cantaloupe/admin

  * username: admin
  * password: islandora

You can access the IIIF interface at: http://localhost:8080/cantaloupe/iiif/2/

### JWT

Islandora 8 uses JWT for authentication across the stack. Crayfish microservices, Fedora, and Drupal all use them.
Crayfish and Fedora have been set up to use a master token of `islandora` to make testing easier. To use it, just set
the following header in HTTP requests:

  * `Authorization: Bearer islandora`

### BlazeGraph (Bigdata)

You can access the BlazeGraph interface at: http://localhost:8080/bigdata/

You have to select the islandora namespace in the [namespaces tab](http://localhost:8080/bigdata/#namespaces) before you can execute queries.

### FITS

You can access the FITS Web Service at http://localhost:8080/fits/

### Matomo

CLAW Playbook installs an instance of the [Matomo](https://matomo.org/) (formally PIWIK) web analytics platform. You can access your instance at: http://localhost:8000/matomo

  * username: admin
  * password: islandora

## Roadmap

Our highest priority moving forward is testing installation on different network topologies (e.g. 2, 3, 4 server setups, etc...)

## Maintainers

* [Jonathan Green](https://github.com/jonathangreen)

