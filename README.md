
This readme is a modified version of [Bob Crutchley's](https://github.com/bob-crutchley/vagrant-template/blob/master/README.md) documentation 


# Vagrant project template
Documentation of a template in making making and managing machines in VirtualBox

## defining guest machines
Guest machines and their properties are stored in the [`guest_machines.yml`](guest_machines.yml) file.
This file contains the properties needed to configure the VMs. 
The guest `name`, `box`, `cpus`, `memory` and `private_ip` must be defined:

```
---
- name: jenkins
  box: centos/7
  cpus: 1
  memory: 2048 
  private_ip: 10.0.10.10
  scripts:
  - init_ssh.bash
- name: deploy
  box: centos/7
  cpus: 1
  memory: 2048 
  private_ip: 10.0.10.11
  scripts:
  - init_ssh.bash
...
```

## Guest Machine Properties
### Required
- **name**  
	The name is how you can interact with the guest via Vagrant, 
	for example to only start the virtual machine called `default` 
	you could run `vagrant up default`
- **box**  
	The name of the box to be provisioned on the guest machine 
	that is either installed locally or downloaded
	from the [Vagrant Cloud](https://app.vagrantup.com/boxes/search)
- **cpus**  
	Amount of cpus to assign to the guest machine
- **memory**  
	Amount of RAM in Megabytes to assign to the guest machine
- **private_ip**  
	Private IP address of the guest machine, 
	which will allow connectivity from the host
  
  ### Optional
- **synced_folders**  
	Folders to syncronise to the guest machine from the host, 
	this could be useful for number of reasons, such as syncing
	a local maven repository:
	```yaml
	---
	- name: default
	  box: centos/7
	  cpus: 2
	  memory: 4096 
	  private_ip: 10.0.10.10
	  synced_folders:
      - host: ~/.m2
        guest: /home/vagrant/.m2
	...
	```
	**Note**: for synced folders the VirtualBox Guest Additions
	plugin must be installed for Vagrant:
	```bash
	vagrant plugin install vagrant-vbguest
	```	
- **package_manager**  
	The supported package managers are `yum`, `apt` and `apk`.
	When you choose a package manager, all packages will be updated
	```yaml
	package_manager: yum
	```
- **packages**  
	Packages that will be installed using the `package_manager` that has been selected
	```yaml
	packages:
	- wget
	- unzip
	- vim
	```
- **scripts**  
	Scripts may be created in the `vagrant_scripts` folder so that
	they can be accessed. To run a script, include the name of the
	file in the scripts list for the guest:
	```yaml
	scripts:
	- startup
	- configure_firewall.sh
	```
- **forwarded_ports**  
	Ports from the guest machines can be forwarded to the host with this property. 
	You may have multiple ports forwarded per guest but careful of clashes:
	```yaml
	forwarded_ports:
	- guest: 9000
	  host: 9000
	- guest: 8080
	  host: 9900
	```
