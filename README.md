# Test task:
## Used: Vagrant, Ansible, Ganglia, Monit

This folder contains next folders:
* [VM1](https://github.com/YevhenDuma/testtask/tree/master/Vagrant): folder with vagrant file for virtual machines and ansible hosts files
* [ansible](https://github.com/YevhenDuma/testtask/tree/master/ansible): folder with ansible roles.


## VM's
### VM1

To start: 
* navigate to Vagrant folder
* run next command
	`vagrant up vm1`


VM1 is using next ports:
* 10022 for ssh
* 10080 for http
* 10443 for https

### VM2

To start: 
* navigate to Vagrant folder
* run next command
        `vagrant up vm2`

VM2 is using next ports:
* 11022 for ssh
* 11080 for http
* 11443 for https

## Ansible
* [vm1.yml](https://github.com/YevhenDuma/testtask/blob/master/ansible/vm1.yml) to configure VM1
* [vm2.yml](https://github.com/YevhenDuma/testtask/blob/master/ansible/vm2.yml) to configure VM2
* [roles](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles): description of roles

### Ansible roles
Next roles defined:
* apache2: to install and configure apache with modules
* app: install and configure custom python application
* common: some common tasks, like apt-get update
* ganglia: install and configure ganglia monitoring tool
* monit: install and configure monit monitoring tool
* sslcert: create self-signed ssl certificate

## Start
To start any vagrant virtual machine you need:
* install VirtualBox
* install Vagrant, tested on vagrant 1.4.3
* navigate to Vagrant directory (VM1 or VM2) and run:
	`vagrant up`

## Tests
To run stress/load tests you will need apache benchmark installed.
Command to run stress/load test:
`ab -c 100 -n 1000000 http://localhost:11080/app`
* -c 100 means 100 concurrency connections
* -n 1000000 means 1M requests
* http://localhost:11080/app - application to to access.

## Monitoring
[Monit](https://localhost:10443/monit)

[Genglia](https://localhost:11443/ganglia)

I've used Ganglia for VM2  and Monit VM1.
Both doesn't require any login information, such as username or password.
Ganglia - role found on githab, README.md still there. Monit partially taken from other githab repo.

## Application 
[URL](https://localhost:11443/app)

I've made simple python application that shows amount of requests since last start. Application is using page /status - to show amount of requests, without incrementing. Port number and bind address configured as variable in main ansible file.

##SSL
There is ansible role that creates self-signed ssl certificate. Certificate details could be changed in vm.yml
