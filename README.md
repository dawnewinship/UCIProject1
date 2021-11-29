# UCIProject1
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on AWS. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML files may be used to install only certain pieces of it, such as Filebeat.

filebeat-playbook.yml

metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly accessible, in addition to restricting unauthorized access to the network.
Load balancers protect the server and integrity of access.  The jump box provides depoloyment of services without manual time-consuming human enterfaces on a set schedule. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
  Filebeat watch for log data, while Metricbeat records teh metrics and statistics from teh sytem and services running on the server. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Ancible  | Gateway  | 10.0.0.106 | Amazon Linux     |
| ELK      | Server   | 10.0.0.191 | Linux/Ubuntu     |
| DVWA1    | Server   | 10.0.0.159 | Amazon Linux     |
| TODO     |          |            |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the those connected through the route table can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
10.0.2.0/24
10.0.0.0/16

Machines within the network can only be accessed by Docker for Containers.
Docker for Containers IP 10.0.0.191

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
|   ELK    |      NO             | 10.0.0.191           |
| Ancible  |      NO             | 10.0.0.106           |
|  DVWA 1  |      NO             | 10.0.0.159           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it avoids human error and can be
set on a standardized maintenance schedule. 

The playbook implements the following tasks:
-name: Configure Elk VM with Docker
  hosts: elkservers
  remote_user: elk
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
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

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Ancible   IP 10.0.0.106         

We have installed the following Beats on these machines:
Filebeat
Metricbeat
Packetbeat

These Beats allow us to collect the following information from each machine:
Filebeat to detect changes to the file system; specifically Apache logs
Metricbeat to detech changes in metrics such as CPU usage as well as SSH login attempts, failed sudo escalations and CPU/RAM statistics
Packetbeat collects packets that pass through NIC, similar to Wireshark and are used to generate a trace of all activity that takes place on the network in the event of 
later forensic analysis needs

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned (although we have used the 
jump box): 

SSH into the control node and follow the steps below:

- Copy the ansible file ansible.cfg

- Run the playbook 
 ansible-playbook install_elk.yml elk
 ansible-playbook install_filebeat.yml webservers
 ansible-playbook install_metricbeat.yml webservers
 
 and navigate to run curl http://10.0.0.8:5601 


All playbooks are in the files for Project1 and can be copied to etc/ansible

Update the ansible yml file to make Ansible run the playbook on a specific machine
The ELK server should be installed on the Gateway and the filebeat on webservers

Navigate to http://10.0.0.8:5601 in order to check that the ELK server is running?

Sudo apt update
Sudo apt-get install docker-ce docker-ce-cli containerd.io
Sudo apt install docker.io
Sudo docker pull sebp/elk

sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk

