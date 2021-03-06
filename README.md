
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk Project Diagram](https://user-images.githubusercontent.com/77945768/117547132-d4a53a80-afe2-11eb-9019-7bfe2aa913e2.jpg)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.

```
---
  - name: Configure Elk VM with Docker
    hosts: elk
    remote_user: RedAdmin
    become: true
    tasks:


    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present
   
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present


    - name: Install Docker python module
      pip:
        name: docker
        state: present


    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
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
```

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting authorized access to the network.
What aspect of security do load balancers protect? What is the advantage of a jump box?
- Load Balancers provide protection against Denial of Service (DDoS) attacks. The advantage of a jump box is to provide access to a user from a single node which allows it to be monitored easily. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- What does Filebeat watch for?
- Filebeat is used to forward and centralize log data. It allows the user to collect log events (ssh logins, sudo commands, new users and groups).
- What does Metricbeat record?_
- Takes metrics and statistics from an operating system and from the services running on it.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web 1  |   Web Sever       |    10.0.0.5        |           Linux       |
| Web 2   |      Web Server    |      10.0.0.7      |            Linux      |
| Web 3   |      Web Server    |        10.0.0.10    |        Linux          |
| Elk  |    Monitoring      |      10.1.0.4      |        Linux          |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 107.185.21.128

Machines within the network can only be accessed by each other.
- Which machine did you allow to access your ELK VM? What was its IP address?_
- Web 1, Web 2, Web 3, and Local Machine have access to Elk VM. 13.90.19.146

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes             | 13.87.186.6 |
|    Elk Server      |      Yes               |          40.71.84.22          |
|        Web 1  |  No                   |           10.0.0.5           |
|    Web 2      |    No                 |           10.0.0.7           |
|     Web 3    |       No              |        10.0.0.10              |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_
- No human error with automation if done correctly. Not to mention you're able to rapidly deploy on a huge scale.

The playbook implements the following tasks:
- Install Docker.io
- Install Python3
- Install Docker python module
- Increase Virtual memory
- Download and launch a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker](https://user-images.githubusercontent.com/77945768/117527481-52385e80-af81-11eb-8be7-71102061233b.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.0.0.5
- Web 2: 10.0.0.7
- Web 3: 10.0.0.10

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filbeat: Detects any changes to the filesystem. Allows easy monitoring of SSH logins, sudo commands, and any groups and user changes.
- Metricbeat: Collects metrics from the operating system and services running on the server. Shows CPU Usage, Memory Usage, and Network IO.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [install-elk.yml](https://github.com/Tamayo-bit/Elk-Project/blob/main/Ansible/install-elk.yml) file to ansible container youthful_buck.
- Update the hosts file to include the Elk Server's Ip under a new category called [Elk] as seen in line 31 and 32.
- Run the playbook, and navigate to http://40.71.84.22:5601/app/kibana to check that the installation worked as expected.


- Which file is the playbook? Where do you copy it?_
  - [Install-Elk.yml](https://github.com/Tamayo-bit/Elk-Project/blob/main/Ansible/install-elk.yml). You would copy it to the Ansible Container in this case youthful_buck.
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
  - [filebeat-config.yml](https://github.com/Tamayo-bit/Elk-Project/blob/main/Ansible/filebeat-config.yml). You go into the config file and edit line 1105 and 1805 to include the Elk server's ip address. Once the ip address has been entered the playbook will need to specify the source location for the filebeat-config.yml so that it only installs onto the Elk Server.
- Which URL do you navigate to in order to check that the ELK server is running?
   -  http://40.71.84.22:5601/app/kibana
- _As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

```
# SSH to Jumpbox
$ sudo docker start youthful_buck
$ sudo docker attach youthful_buck
# We will want to locate where all our files are
$ cd /etc/ansible
$ ls
ansible.cfg files hosts install-elk.yml pentest.yml roles
$ cd files
filebeat-config.yml filebeat-playbook.yml metricbeat-config.yml metricbeat-playbook.yml
# Now that we know the locations of our files within the docker container we can exit and begin to download the files and upload them to Github repository.
$ exit
# Clone repository
$ git clone https://github.com/Tamayo-bit/Elk-Project.git
$ cd /Elk-Project
$ mkdir Ansible
# Move Playbooks and hosts files from the docker containter into /Elk-Project directory
$ sudo docker cp youthful_buck:/etc/ansible/hosts /home/RedAdmin/Elk-Project/Ansible
$ sudo docker cp youthful_buck:/etc/ansible/files/filebeat-config.yml /home/RedAdmin/Elk-Project/Ansible
$ sudo docker cp youthful_buck:/etc/ansible/files/filebeat-playbook.yml /home/RedAdmin/Elk-Project/Ansible
$ sudo docker cp youthful_buck:/etc/ansible/files/metricbeat-config.yml /home/RedAdmin/Elk-Project/Ansible
$ sudo docker cp youthful_buck:/etc/ansible/files/metricbeat-playbook.yml /home/RedAdmin/Elk-Project/Ansible
# This pulls the playbooks, configs, and host file from the docker containter.
$ git add .
$ git commit -m "First commit"
$ git push
# The terminal will request your github username and password to push the files.
# Next we need to ensure that the hosts file contains the proper ips to ensure the playbooks will run properly. You'll want to start and attach to the docker container once again.
$ cd /etc/ansible
$ sudo nano hosts
# Ensure that you remove the # next to [webservers] and add your ips for your vms under line 24. Once this is done you'll need to go create a new line under 29 and add [elk]. Under the [elk] line you'll add the up for your elk machine. Refer to hosts file for visual. Once the ips are added to the proper sections you can run the playbooks as followed:
$ cd /etc/ansible
$ ansible-playbook install_elk.yml 
$ cd /etc/ansible/files
$ ansible-playbook filebeat-playbook.yml
$ ansible-playbook metricbeat-playbook.yml
# If succesfully done run the following command:
$ curl http://10.1.0.0.4:5601
# This should print a HTML onto the console showing that it is up and running. You can then visit the site at http://40.71.84.22:5601/app/kibana to ensure everything is up and running.

```
