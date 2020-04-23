# MSC Geospatial Development Environment
This repository is used to manage the Vagrantfile used to create a quick
development environment for students and MSC Geospatial and Open Data Systems
(GODS) team members. We use [Vagrant](https://www.vagrantup.com/intro/index.html)
and [VirtualBox](https://www.virtualbox.org/) to create a virtual
machine (VM) that closely resembles the existing architecture used to deploy
many of our applications.

The VM created will contain the following software:
* Latest GDAL version available on [unbuntugis-unstable](https://launchpad.net/~ubuntugis/+archive/ubuntu/ubuntugis-unstable)
* Latest MapServer version available on [unbuntugis-unstable](https://launchpad.net/~ubuntugis/+archive/ubuntu/ubuntugis-unstable)
* Latest [GDAL Python bindings](https://pypi.org/project/GDAL/) available via PyPI
* Latest Elasticsearch `7.x` version from Elasticsearch [APT repository](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html#deb-repo)

## Installation Guide
### Download Vagrant and VirtualBox
In order to setup the VM, you will need to download the following software for
the operating system of your choice:
* [Vagrant](https://www.vagrantup.com/downloads.html)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

#### Mac OS X
- compatible versions of Vagrant are required.  Below are working combinations:
  - Vagrant 2.2.7 and VirtualBox 6.1

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
for Elasticsearch to run properly in the VM. Please consider providing at
minimum 2048 MB of memory to your VM**.
#### Linux / Mac OS X
Open up a Terminal and `cd` into the local directory you chose above.
Run `vagrant up`. This will start up the VM for the first time and run through
the provisioning process.
#### Windows
You may need to enable VT-x in your computer's BIOS. If it is not enabled,
VirtualBox will raise an error during the next step.

Using the `Command Prompt` navigate to the local directory you chose above.
Run `vagrant up`. This will start up the VM for the first time and run through
the provisioning process.

## Usage
### Accessing MapServer and Elasticsearch via your host computer
Following the initial boot of your VM, both MapServer and Elasticsearch will
be available via your browser on your host computer:
* Elasticsearch: http://localhost:9201
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
folder is also created in the same directory as the Vagrantfile which is made
available on the VM at `/home/vagrant/shared`.

It is a good idea to clone any repositories in this directory as it will allow
you to access them from your favourite IDE on your local computer.

### Basic Vagrant commands
```bash
# turn on VM
vagrant up

# pause VM
vagrant suspend

# turn off VM
vagrant halt

# destroy VM
vagrant destroy

# rerun the initial provisioning careful!
vagrant provision
```

For more information on using the Vagrant CLI, please read the
[Vagrant CLI documentation](https://www.vagrantup.com/docs/cli/)
