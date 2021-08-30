# Project1-UWA
My first project for uni
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Diagrams/(Liam Pickering) Unit 12 Homework.png) This is the virtual network diagram

(All_Images_for_project.pdf) The images shwoing data being push into the ELK server

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ElkDocker file may be used to install only certain pieces of it, such as Filebeat.

To start the elk server, run ansible-playbook and the file bellow.
  - Enter the playbook file. - ElkDocker.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly secure, in addition to restricting external IP's to the network.
What aspect of security do load balancers protect? What is the advantage of a jump box?
The aspect of security that load balancers protect is the flow of informatiuon between servers and the endpoint device. The load balancer helps servers move data around efficiently, and prevents server overloads.
The advantage of a jump box is the security it provides, it is a secure computer that all system administrators first connect to before launching any admin task or used as an origin point to connect to other servers, trusted or untrusted environments.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system logs.
What does Filebeat watch for? Filebeat watches/monitors the log files or locations that is specified, collects log events and forwards them either to the Elasticsearch or logstash for indexing. Put simply filebeat is a lightweight shipper for pushing forward and centralizing log data.
What does Metricbeat record? Metricbeat periodically collects metrics from the operating system and from services running on the server. Metricbeat takes the metrics and stats that it collects and ships the output that is specified, with either elasticsearch or logstash.

The configuration details of each machine may be found below.

| Name                  | Function               | IP Address | Operating System     |
|-----------------------|------------------------|------------|----------------------|
| Jump-Box-Provisioner  | Gateway                | 10.0.0.4   | Linux (ubuntu 18.04) |
| ELKvm                 | ELK/Kibana             | 10.1.0.4   | Linux (ubuntu 18.04) |
| Web-1                 | Filebeat/Metricbeat VM | 10.0.0.5   | Linux (ubuntu 18.04) |
| Web-2                 | Filebeat/Metricbeat VM | 10.0.0.6   | Linux (ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
106.69.51.79 - my IP address

Machines within the network can only be accessed by the personal machine.
Which machine did you allow to access your ELK VM? What was its IP address? The Jump-Box-Provisioner -> ansible container is allowed access to the ELK VM, 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses                    |
|----------|---------------------|-----------------------------------------|
| Jump Box | Yes/No              | 106.69.51.79 10.0.0.5 10.0.0.6 10.1.0.4 |
| ELK VM   | No                  | 10.0.0.4                                |
| Web-1    | No                  | 10.0.0.4                                |
| Web-2    | No                  | 10.0.0.4                                |

### Elk Configuration

What is the main advantage of automating configuration with Ansible?
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows system admininstrators to automate away the drudgery from their daily tasks. Doing this frees up time for them to focus on other important tasks for the business.

The playbook implements the following tasks:
- Install Docker
- Install pip3
- Install Docker python module
- Set the vm.max_map_count to 262144 and set the state to present and reload to yes
- Download and launch a docker elk docker container and use the image sebp/elk:761 set the state to started and the restart policy to always and lastly set 3 publiched ports, 5601:5601, 9200:9200 and 5044:5044

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used for forwarding and centralizing log data. Filebeat monitors the log files or locations that you specify, collects log events and forwards them either to Elasticsearch or logstash for indexing. This is used to track what log files are going to and from virtual machines. 
- Metricbeat is used to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and stats, collects them and ships them to the output that is specified, such as Elasticsearch or logstash. This can be used to track what processes are running on an operating system or server, and from there used to determine whether a process is required or if a process is malicious or not.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to heartbeat-playbook.yml .
- Update the heartbeat-playbook.yml file to include the ability to monitor the availbility of the servers.
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it? The playbook file I copied was filebeat-playbook.yml, and I turned it into heartbeat-playbook.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? You update the auditbeat-playbook.yml, change the hosts option from webservers to the IP of the VM that you want to download it too, for example I'll be using 10.0.0.6
- _Which URL do you navigate to in order to check that the ELK server is running? http://40.118.185.16:5601/app/kibana#/home/tutorial/auditbeat (When restarting the machines, the IP address will change)

- To run the playbooks
-	ansible-playbook filebeat-playbook.yml
-	ansible-playbook metricbeat-playbook.yml
-	ansible-playbook heartbeat-playbook.ym

- To edit the playbooks
-	nano /etc/ansible/roles/heartbeat-playbook.yml
-	nano /etc/ansible/roles/filebeat-playbook.yml
-	nano /etc/ansible/roles/metricbeat-playbook.yml
