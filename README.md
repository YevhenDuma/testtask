# Test task:
## Used: Vagrant, Ansible, Ganglia, Monit

This folder contains next folders:
* [Vagrant](https://github.com/YevhenDuma/testtask/tree/master/Vagrant): folder with vagrant file for virtual machines and ansible hosts files
* [ansible](https://github.com/YevhenDuma/testtask/tree/master/ansible): folder with ansible roles.


## VM's
### VM1

To start: 
* navigate to Vagrant folder
* run next command
	`vagrant up vm1`


Roles:
* common
* apache2
* sslcert
* ganglia


VM1 is using next ports:
* 10022 for ssh
* 10080 for http
* 10443 for https

### VM2

To start: 
* navigate to Vagrant folder
* run next command
        `vagrant up vm2`


Roles:
* common
* apache2
* sslcert
* app


VM2 is using next ports:
* 11022 for ssh
* 11080 for http
* 11443 for https


## Ansible
* [vm1.yml](https://github.com/YevhenDuma/testtask/blob/master/ansible/vm1.yml) to configure VM1
* [vm2.yml](https://github.com/YevhenDuma/testtask/blob/master/ansible/vm2.yml) to configure VM2
* [roles](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles): description of roles

### Ansible roles
Next roles defined and described:
* [apache2](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles/apache2): to install and configure apache with modules
* [app](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles/app): install and configure custom python application
* [common](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles/common): some common tasks, like apt-get update
* [ganglia](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles/ganglia): install and configure ganglia monitoring tool
* [sslcert](https://github.com/YevhenDuma/testtask/tree/master/ansible/roles/sslcert): create self-signed ssl certificate

## Start
To start any vagrant virtual machine you need:
* install VirtualBox
* install Vagrant, tested on vagrant 1.4.3
* navigate to Vagrant directory and run:
	`vagrant up <vm1 or vm2>`

## Tests
To run stress/load tests you will need apache benchmark installed.
Command to run stress/load test:
`ab -c 100 -n 1000000 http://localhost:11080/app`
* -c 100 means 100 concurrency connections
* -n 1000000 means 1M requests
* http://localhost:11080/app - application to to access.

To check results:
* access [ganglia monitorint tool](https://localhost:11443/ganglia)
* in metrics find app_requests

![ScreenShot](https://cloud.githubusercontent.com/assets/4558068/9425688/93176818-4922-11e5-8124-e04068d884d8.png)

## Monitoring
[Genglia](https://localhost:10443/ganglia)

I've used Ganglia.  It doesn't require any login information, such as username or password.
This role I've found on githab, README.md still there. 
I've created custome module for ganglia, that call's url, parce results and sends them to ganglia server. Ganglia shows delta - difference between previous value and current. Threshold defined as variables inside vm1.yml


## Application 
[URL](https://localhost:11443/app)

I've made simple python application that shows amount of requests since last start. Application is using page [/status](https://localhost:11443/app/status) - to show amount of requests, without incrementing. Port number and bind address configured as variable in main ansible file.

Ansible will install application as upstart. 
To check status run:
`status app`
To restart:
`restart app`


##SSL
There is ansible role that creates self-signed ssl certificate. Certificate details could be changed in vm.yml
Both VM's using port 443(https).
