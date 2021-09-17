## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!Images/Scott_Gannon_ELK.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML playbook file may be used to install only certain pieces of it, such as Filebeat.

  -install-elk.yml
  -filebeat-config.yml
  -filebeat-playbook.yml
  -metricbeat-config.yml
  -metricbeat-playbook.yml


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- What aspect of security do load balancers protect?
	- Load balancers protect the Availability aspect of security by monitoring traffic and shifting that traffic so that no one system gets over-burdened. 
- What is the advantage of a jump box?
	- The jumpbox is an administrative-use-only access point to hardened networks. These networks require all admin tasks to originate from that one box. Having one point of access makes monitoring and controlling access easier.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.
- What does Filebeat watch for?
	- Filebeat collects data about the file system and watches for changes
- What does Metricbeat record?
	- Metricbeat records machine (OS and network service) metrics and stats, such as uptime

The configuration details of each machine may be found below.

|         Name         |  Function  |   IP Address   |    Operating System   |
|:--------------------:|:----------:|:--------------:|:---------------------:|
| Jump-Box-Provisioner | Gateway    | 40.83.249.129  | Linux(ubuntu 20.04.3) |
| ELKStack-1           | ELK Server | 40.122.109.116 | Linux(ubuntu 20.04.3) |
| Web-1                | Server     | 104.42.77.70   | Linux(ubuntu 20.04.3) |
| Web-2                | Server     | 104.42.77.70   | Linux(ubuntu 20.04.3) |
| Web-3                | Server     | 104.42.77.70   | Linux(ubuntu 20.04.3) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 71.65.153.55

Machines within the network can only be accessed by SSH from Jump Box.
- Which machine did you allow to access your ELK VM? What was its IP address?
	- Jump-Box-Provisioner; 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses   |
|----------------------|---------------------|------------------------|
| Jump-Box-Provisioner | Yes                 | 71.65.153.55           |
| ELKStack-1           | Yes                 | 71.65.153.55; 10.0.0.4 |
| Web-1                | No                  | 10.0.0.4               |
| Web-2                | No                  | 10.0.0.4               |
| Web-3                | No                  | 10.0.0.4               |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?
	- They focus on brining a server to a certain state of operation meaning that once the state of a server is documented, that code can be run to spin up as many servers as needed within minutes.

The playbook implements the following tasks:
- Install docker.io
- Install pip-3
- Install docker module
- Increase vitural memory usage
- Download and install docker ELK container image sebp/elk:761

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.
	- Filebeat collects log files and log events which we can use to track log activity which we can use to track brute force attacks or other attempts to access our system
	- Metricbeat collects metrics such as CPU usage, Memory Usage, Load, Inbound & Outbound speeds, ect. We can use this information to see if our load balancer is working and if there is an usually high amount of traffic to the servers which could indicate a DoS attack

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to the Ansible container.
- Update the hosts file to include the elk host group and, in that group, the ELKStack-1 VM IP address
- Run the playbook, and navigate to http://40.122.109.116:5601/app/kibana to check that the installation worked as expected.

Answer the following questions to fill in the blanks:
- Which file is the playbook? Where do you copy it?
	- install-elk.yml; /etc/ansible/ on the Ansible container
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
	- /ect/ansible/hosts file; the groups within the host file are called when running the playbooks. The 'elk' group is called to install the ELK server on while the 'webserver' group is called to install Filebeat
- Which URL do you navigate to in order to check that the ELK server is running?
	-http://40.122.109.116:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.
Ansible-playbook install-elk.yml