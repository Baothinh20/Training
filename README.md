# Practice 1
# OpenStack AIO
## Author: Tran Bao Thinh
## Table of contents
### I.Create Ubuntu virtual machine
### II.Setup OpenStack AIO inside VM with Kolla
## I.Create Ubuntu virtual machine
### Installing
- Ubuntu download link: https://ubuntu.com/download/desktop

- Virtual box download link: https://www.virtualbox.org/wiki/Downloads

Note: - My ubuntu version is 22.04 and my virtual box version is 6.1.34

### Setup
- Install virtual box and run:



## II.Setup OpenStack AIO inside VM with Kolla
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
sudo apt install python3-venv
```

- create virtual environment

```bash
$ python3 -m venv /path/to/venv
$ source /path/to/venv/bin/activate
```

### 3. Install Ansible and Kolla-ansible (using a virtual environment)

Note: using a virtual environment before run.

- Install Ansible

```bash
pip install 'ansible>=4,<6'
```
