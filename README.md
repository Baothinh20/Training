# Practice 1
# OpenStack AIO
## Author: Tran Bao Thinh
## Table of contents
### I. Overview
#### A.Openstack
#### B.Kolla-Ansible
### II.Create Ubuntu virtual machine
### III.Setup and Deloy OpenStack AIO inside VM with Kolla
## I. Overview
1. Overview:
 - OpenStack is an open source platform that uses pooled virtual resources to build and manage private and public clouds
    - It is mostly deployed as infrastructure-as-a-service (IaaS)
    
    - The software platform consists of interrelated components that control diverse, multi-vendor hardware pools of processing, storage, and networking resources           throughout a data center.
    
    - OpenStack began in 2010 as a joint project of Rackspace Hosting and NASA. As of 2012, it was managed by the OpenStack Foundation, a non-profit corporate entity       established in September 2012[3] to promote OpenStack software and its community.
    - 
2. Core Components:

  - Nova: The OpenStack project that provides a way to provision compute instances (aka virtual servers). Nova supports creating virtual machines.

  - Neutron:  It is the network service within OpenStack. Manages & Performs all network configurations, operations.

  - Glance: Maintains and manages server images in Cloud

  - Horizon: The implementation of OpenStack’s Dashboard, which provides a web based user interface to OpenStack services including Nova, Swift, Keystone, etc.

  - Cinder: It is the block storage service of OpenStack, providing virtual storage for virtual machines in the system.

  - Swift: The object storage component of OpenStack, is a scalable and distributed storage system.

  - Keystone: It is the authentication and authorization system of OpenStack.

## II.Install and Create Ubuntu/virtual machine
### A. Installing
- Ubuntu download link: https://ubuntu.com/download/desktop

- Virtual box download link: https://www.virtualbox.org/wiki/Downloads

Note: - My ubuntu version is 20.04 and my virtual box version is 6.1.34

### B. Settings for KOLLA-ANSIBLE

  - Kolla provides Docker containers and Ansible playbooks to provide production-ready containers and deployment tools for operating OpenStack clouds.

  - System Requirements:
  
    - Operating System: Ubuntu
    
    - Desktop Hypervisior: VirtualBox
    
  - Virtual machine requirement:
  
    - 2 network interfaces:

    - 8GB main memory
    - 2 disk with 20GB

## III.Setup and Deloy OpenStack AIO inside VM with Kolla ( using a virtual environment )

### 1. Update and Install dependencies

 ```bash
 $ sudo apt update 
 ```

 ```bash
 $ sudo apt install python3-dev libffi-dev gcc libssl-dev
 ```

### 2. Setup and create virtual environment
- Install the virtual environment dependencies (Using virtualenv)

 ```bash
 $ sudo apt install python3-venv
 ```

- create virtual environment

 ```bash
 $ python3 -m venv /path/to/venv
 $ source /path/to/venv/bin/activate
 ```
- Check the last version:

 ```bash
 $ pip install -U pip
 ```

### 3. Install Ansible and Kolla-ansible

Note: using a virtual environment before run.

- Install Ansible

 ```bash
 $ pip install 'ansible>=4,<6'
 ```

- Install Kolla-ansible:

 ```bash
 $pip install git+https://opendev.org/openstack/kolla-ansible@master
 ```

- Install Ansible Galaxy dependencies:

 ```bash
 $ kolla-ansible install-deps
 ```

### 3. Config Ansible and Kolla-ansible 

- Create the /etc/kolla directory:

 ```bash
 $ sudo mkdir -p /etc/kolla
 $ sudo chown $USER:$USER /etc/kolla
 ```

- Copy globals.yml and passwords.yml to /etc/kolla directory:

 ```bash
 $ cp -r /path/to/venv/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
 ```

- Configure Ansible:

 ```bash
 $ sudo mkdir -p /etc/ansible
 $ config="[defaults]\nhost_key_checking=False\npipelining=True\nforks=100"
 $ echo -e $config >> /etc/ansible/ansible.cfg
 ```

- Check configurations:

 ```bash
 $ ansible -i all-in-one all -m ping
 ```

- Create diskspace partition for Cinder:

 ```bash
 $ sudo pvcreate /dev/sdb
 $ sudo vgcreate cinder-volumes /dev/sdb
 ```

- Generate Passwords for Kolla:

 ```bash
 $ kolla-genpwd
 ```

- Configure globals.yml:

 ```bash
 kolla_base_distro: "ubuntu"

 network_interface: enp0s3
 neutron_external_interface: enp0s8
 kolla_internal_vip_address: 10.0.2.15

 nova_compute_virt_type: "qemu"

 enable_haproxy: "no"

 enable_cinder: "yes"
 enable_cinder_backup: "no"
 enable_cinder_backend_lvm: "yes"
 ```
### 4. DEPLOY OPENSTACK
 - Bootstrap servers with kolla deploy dependencies

 - Do pre-deployment checks for hosts

 - Pull Images to VM
 
 - Deploy

### 5. Using Openstack
