# Project1

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![https://github.com/adboler/Project1/blob/main/Cloud_security_diagram2.PNG](Images/Cloud_security_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - ---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azdmin
  become: true
  tasks:

  - name: Install docker.io
    apt:
     update_cache: yes
     force_apt_get: yes
     name: docker.io
     state: present

  - name: install python3-pip
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  - name: Install Docker module
    pip:
      name: docker
      state: present

  - name: Increase virtual memory
    command: sysctl -w vm.max_map_count=262144

  - name: Use more memory
    sysctl:
      name: vm.max_map_count
      value: 262144
      state: present
      reload: yes

  - name: download and launch a docker elk container
    docker_container:
       name: elk
       image: sebp/elk:761
       state: started
       restart_policy: always

       published_ports:

          - 5601:5601

          - 9200:9200

          - 5044:5044



  - name: Enable service docker on boot
    systemd:
        name: docker
        enabled: yes

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _____, in addition to restricting _____ to the network.
- Load balancers protects the system from DDoS attacks by shifting attack traffic.
The advantage of a jump box is to give access to the user from a single node that can be secured and monitored.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- Filebeat watches for any information in the file system which has been changed and when it has.
Metricbeat takes the metrics and statistics that collects and ships them to the output you specify

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| VM-1     | Server   | 10.0.0.7   | Linux            |
| VM-2     | Server   | 10.0.0.6   | Linux            |
| VM-3     | Server   | 10.0.0.8   | Linux            | 
| ELKserver| Server   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_

Machines within the network can only be accessed by _____.
- Jump Box VM: VNET IP 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Home IP              |
| VM-1     | No                  | 10.0.0.4             |
| VM-2     | No                  | 10.0.0.4             |
| VM-3     | No                  | 10.0.0.4             |
| ELKserver| No                  | 10.0.0.4             |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Advantage is that you can put commands into multiple servers from a single playbook

The playbook implements the following tasks:
-Install: docker.io
-Install: python-pip
-Install: docker
-Command: sysctl -w vm.max_map_count=262144
-Launch docker container: elk

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![https://github.com/adboler/Project1/blob/main/docker_ps.PNG](main/docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _VM-1 10.0.0.7 VM-2 10.0.0.6 VM-3 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat and metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects the changes done Metric beat collects metrics and statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

- /etc/ansible/filebeat-config.yml
- edit the /etc/ansible/host file to add webserver/elkserver ip addresses
- _Which URL do you navigate to in order to check that the ELK server is running? www.publicip:5601 (Kibana)
-
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
    -ansible-playbook filebeat-playbook.yml
