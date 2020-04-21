# MSC Geospatial Development Environment
This repository is used to managed to Vagrantfile used to create a quick
development environment for students and MSC Geospatial and Open Data Systems
(GODS) team members. We use [Vagrant](https://www.vagrantup.com/intro/index.html)
and [VirtualBox](https://www.virtualbox.org/) to create a virtual
machine (VM) that closely resembles the existing architecture used to deploy
many of our applications.

The VM created will contain the following software:
* Latest GDAL version available on [unbuntugis-unstable](https://launchpad.net/~ubuntugis/+archive/ubuntu/ubuntugis-unstable).
* Latest MapServer version available on [unbuntugis-unstable](https://launchpad.net/~ubuntugis/+archive/ubuntu/ubuntugis-unstable).
* Latest [GDAL python bindings](https://pypi.org/project/GDAL/) available via PYPI.
* Latest ElasticSearch `7.x` version from ElasticSearch [APT respository](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html#deb-repo).

## Installation Guide
### Download Vagrant and VirtualBox
In order to setup the VM, you will need to download the following software for
the operating system of your choice:
* [Vagrant](https://www.vagrantup.com/downloads.html)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
### Setup
The following steps will get you started with your development environment.
#### Get Vagrantfile
Either clone this directory or download the Vagrantfile to your computer.
Place the Vagrantfile inside a local directory of your choosing
(i.e. `~/devel/gods-dev-vm/`).
### Booting up the VM
Prior to booting the VM, you may want to edit the Vagrantfile in your
favourite text editor to modify the initial amount of CPUs and memory (RAM)
used by your machine. To change these values, open the file and look for:
```
config.vm.provider "virtualbox" do |v|
	v.memory = 2048
	v.cpus = 2
```
Modify the existing values to suit your needs based off of your existing
hardware.

**IMPORTANT: In many cases, 1024 MB of memory is insufficient
for ElasticSearch to run properly in the VM. Please consider providing at
minimum 2048 MB of memory to your VM**.
#### Linux / Mac OS X
Open up a Terminal and `cd` into the local directory you chose above.
Run `vagrant up`. This will start up the VM for the first time and run through
the provisioning process.
#### Windows
Using the `Command Prompt` navigate to the local directory you chose above.
Run `vagrant up`. This will start up the VM for the first time and run through
the provisioning process.

## Usage
### Accessing MapServer and ElasticSearch via your host computer
Following the initial boot of your VM, both MapServer and ElasticSearch will
be available via your browser on your host computer:
* ElasticSearch: http://localhost:9201
* Mapserver: http://localhost:8888/cgi-bin/mapserv?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetCapabilities
### Accessing your VM via SSH
#### Linux / Mac OS X
To access your VM via your host computer, open a terminal window and navigate
to the directory that contains your Vagrantfile and run `vagrant ssh`.
#### Windows
With `Command Prompt` open and in the directory containing your Vagrantfile
run `vagrant ssh`.

It is also possible to access your VM with
[MobaXTerm](https://mobaxterm.mobatek.net/). To do so, open `Command Prompt`
and navigate to your local directory containing the `Vagrantfile` and run:
```
vagrant ssh-config > vagrant-ssh
```
Open MobaXTerm and start a local session, `cd` to the local directory
containing the Vagrantfile and run:
```
ssh -F vagrant-ssh default
```
You should now be connected to your VM via SSH.
### Using the shared folder
Using the shared folder allows you to easily access files between your local
computer and the VM. When running `vagrant up` for the first time, a `shared`
folder is also created in the same directory as the Vagrantfile. This folder
is also available at `/home/vagrant/shared` on your VM. It is a good idea
to clone any repositories in this directory as it will allow you to access
them from your favourite IDE on your local computer.
### Basic Vagrant commands
To turn your VM on:
```
vagrant up
```

To pause your VM:
```
vagrant suspend
```
To turn off your VM:
```
vagrant halt
```
To destroy the VM:
```
vagrant destroy
```
To rerun the initial provisioning (**be careful!**):
```
vagrant provision
```
For more information on using the Vagrant CLI, please read the
[Vagrant CLI documentation](https://www.vagrantup.com/docs/cli/)
