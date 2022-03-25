# ELK-Stack-Project
Project 1 - ELK Stack Project

## Fill in this as your project documentation

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Path to my diagram](Diagrams/NETWORKDIAGRAM.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - [Playbook File](Ansible/install-Elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly flexible, in addition to restricting access to the network.
- _What aspect of security do load balancers protect? What is the advantage of a jump box?_
  - Load balancers protect the principle of availability in the CIA triad. The purpose of load balancers is to manage the incoming traffic on a network and distribute it across the devices it is connected to. In our case, the load balancer can send traffic to 3 different WebVMs. We tested the effectiveness of this by manually stopping WebVMs and seeing that DVWA was still accessible regardless of which WebVM was still active. The advantage of a jump box is that it limits the public IP access of my virtual networks. When only the jump box or only a few devices on a virtual network have public IPs, they are more protected from external threats that probably wouldn't know the internal private IP information about other devices on why network. This is why our VM resources are primarily accessed first through our jump box.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system statistics.
- _What does Filebeat watch for?_
  - Filebeat watches for changes and generates log files for them.
- _What does Metricbeat record?_
  - Metricbeat records system statistics for standard systems or containers.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name               	| Function          	| IP Address 	| Operating System 	|
|--------------------	|-------------------	|------------	|------------------	|
| JumpBoxProvisioner 	| Gateway & Ansible 	| 10.0.0.4   	| Linux            	|
| Web1              	| DVWA container    	| 10.0.0.5   	| Linux            	|
| Web2              	| DVWA container    	| 10.0.0.6   	| Linux            	|
| Web3              	| DVWA container    	| 10.0.0.7   	| Linux            	|
| ElkServer          	| Elk and Kibana    	| 10.1.0.4   	| Linux            	|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from my IP address: 50.5.172.246

Machines within the network can only be accessed by JumpBoxProvisioner (20.127.142.85).

A summary of the access policies in place can be found in the table below.

| Name               	| Publicly Accessible 	| IP Address   	|
|--------------------	|---------------------	|--------------	|
| JumpBoxProvisioner 	| Yes                 	| 50.5.172.246 	|
| Web1              	| No                  	| 10.0.0.4     	|
| Web2              	| No                  	| 10.0.0.4     	|
| Web3              	| No                  	| 10.0.0.4     	|
| ElkServer          	| Yes                 	| 50.5.172.246 	|
| Load Balancer      	| Yes                 	| 50.5.172.246 	|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage of automating configuration with Ansible is that all the operations can be outlined and completed without manual effort. This allows us to quickly and easily see what's being done so we can add to or remove processes if we were to perform similar configurations in the future. It also allows us to easily set certain settings like enabling docker on restart and boot.

The playbook implements the following tasks:
- Install docker.io
- Install python
- Install python client for docker
- Increase available virtual memory for Elk
- Downloads the Elk container for docker
- Starts container, then enables the container to restart automatically when restarting the VM

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[Screenshot of docker ps output](Images/Day1-4.0ensurecontainerisrunning.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1 - 10.0.0.5
- Web2 - 10.0.0.6
- Web3 - 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- _In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
  - Filebeat collects logs on files and records changes. Metricbeat collects statistical data on processes for the machines. For example, we can use Metricbeat to view the percentage (%) CPU usage. We can see this data be navigating to the Kibana dashboard and viewing Filebeat or Metricbeat data.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files.
- Update the filebeat-config.yml file to include `hosts: ["10.1.0.4:9200"]` on line 1105 and `host: "10.1.0.4:5601"` on line 1805
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?_
  - The playbook file is filebeat-playbook.yml ; its directory location is in /etc/ansible/roles/
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
  - In order to make Ansible run the playbook on a different machine, you would need to update the filebeat-playbook.yml file. To specify which machine to install the Elk server versus Filebeat, you would need to change the host in the playbook to as well as update filebeat-config.yml with the correct private IP address (in the previous section, we set this as 10.1.0.4 in lines 1105 and 1805 of filebeat-config.yml).
- _Which URL do you navigate to in order to check that the ELK server is running?_
  - http://52.190.194.240:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
ansible-playbook filebeat-playbook.yml
