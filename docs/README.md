
<!---TODO: 
***Would like to be able to clone this repository and build***

- How to tag image with ansible-builder.. 
- Build tests for the container image build process
  - playbooks to test out the newly added collections/python modules
-->

# Building Custom Execution Images
This is a prodecural document for building custom Ansible Execution Images. 

## Description
The new architecture for Ansible Automation Platform 
requires the use of container images to run Ansible automation 
code.  Ansible Automation Platform (AAP) installation bundle comes with three different
Red Hat supported container images, for an working OOBE.

- [Default Execution Environment](registry.redhat.io/ansible-automation-platform-22/ee-supported-rhel8:latest)
- [Minimial Execution Environment](registry.redhat.io/ansible-automation-platform-22/ee-minimal-rhel8:latest)
- [Ansible Engine 2.9 Execution Environment](registry.redhat.io/ansible-automation-platform-22/ee-29-rhel8:latest)

In order for AAP to be able to run the Ansible automation code, the container images
should include all of the Red Hat supported Ansible collections and Python libraries required 
for an OOB Experience.

This document covers the use cases that require addititions of other internally developed
or community collections to be added to the Execution Images to allow the consumption
of the non-supported Red Hat collections.

<!--
Because there are many available collections to use via Ansible Galaxy,
and many of those collections are created by the Ansible Community, 
the included container images are limited to only the Red Hat supported/published 
collections.  Because of this, there may be some requirements to add additional 
collections and/or plugins to the container images in order to allow 
for consistent end-to-end runtimes between local development environments, and
environments run from different sources.  Some of these sources could be CICD, 
AAP, and other environments that run the Ansible code without the option to 
download and install collections during runtime. 

-->

   
## TOC
- [Requirements](#requirements)
- [Developer Environment](#developer-environment)
- Container images
- Developer Environment
- Build Image
- Push Image
- virtualenv(if prefer to keep python environments clean)

## Requirements
- Linux
  - Podman engine
  - Docker Engine
- Python3.6 (removed) - yum remove python3.6 
- Python3.9 - yum install python39

- python39-devel - yum install python39-devel
- Ansible Builder - pip install ansible-builder


## Getting Started

### Developer Environment
Virtual Environment
- virtualenv
PIP
- pip3
Python 
- Python >=3.8
Ansible Runner
- ansible-runner
Ansible Builder
- ansible-builder
Ansible Navigator
- ansible-navigator
<!--
- Build Requirments - 
	- virtualenv
	- pip3 (for python3.9)
	- build-requirements.txt (available in repository...)
- Python Version
-->

### Verify Python Version

```console
[root@workstation1]# python3 --version

Python 3.9.7
```


### Login To Registry
Login registry.redhat.io

```console
[root@workstation1]# podman login registry.redhat.io
Username: rhat-email@customername.com
Password: 
Login Succeeded!
```
### Create Python Virtual Environment
Create an virtual environment if the machine is being used by multiple developers, 
or to avoid any python package installation conflicts that could over-write any
other python packages that were specific to something other than building
Ansible execution environment images.

### Create Virtual ENvironment

```console
[root@workstation1]# python3 -m virtualenv venv

created virtual environment CPython3.9.7.final.0-64 in 2705ms
  creator CPython3Posix(dest=/root/execution-environments/venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/root/.local/share/virtualenv)
    added seed packages: pip==22.2.2, setuptools==65.3.0, wheel==0.37.1
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
```

### Activate Virtual Environment

```console
[root@workstation1]# . venv/bin/activate
(venv) [root@workstation1]#

```

### Install Python Requirements for Ansible Builder

```console
(venv) [root@workstation1]# pip install -r builder-requirements.txt

Installing collected packages: ansible,.. .
..
Successfully installed MarkupSafe-2.1.1 Parsley-1.3 ansible-builder-1.1.0 ansible-navigator-2.2.0 ansible-runner-2.2.1 attrs-22.1.0 bindep-2.11.0 cffi-1.15.1 distro-1.8.0 docutils-0.19 jinja2-3.1.2 jsonschema-4.16.0 lockfile-0.12.2 onigurumacffi-1.2.0 packaging-21.3 pbr-5.11.0 pexpect-4.8.0 ptyprocess-0.7.0 pycparser-2.21 pyparsing-3.0.9 pyrsistent-0.18.1 python-daemon-2.3.1 pyyaml-6.0 requirements-parser-0.5.0 six-1.16.0 types-setuptools-65.5.0.1 tzdata-2022.5
```
### Make changes to the Collections 

```console
(venv) [root@workstation1]# vim requirements.yml
```
```yaml
---
collections:
- redhat_cop.controller_configuration
- awx.awx

```

### Make changes to the Python Packages
```console
```

### Build the image
```console
(venv) [root@workstation1]# ansible-builder build -v 3

Ansible Builder is building your execution environment image. Tags: 
File context/_build/requirements.yml is already up-to-date.
File context/_build/requirements.txt is already up-to-date.
Rewriting Containerfile to capture collection requirements
Running command:
  podman build -f context/Containerfile context
  ...
  ..
  .

```
### Deactivate the Virtual Environment
```console

(venv) [root@workstation1]# deactivate


```
### Tag the image (should be done at the ansible-builder CLI..)
```console
[root@workstation1]# podman images


```

### Get the image 
```console
[root@workstation1]# podman push 

```
### Test the image using ansible-runner
```console
[root@workstation1]# ansible-runner run ./ -p hello.yml --container-image <new-image-name:dev>

```
### Test the image using ansible-navigator
```console
[root@workstation1]# ansible-navigator run hello.yml --eei localhost/custom-python-ee:dev

<Press 0>
  Play name	 Ok  Changed     Unreachable       Failed    Skipped     Ignored     In progress       Task count            Progress
0│localhost       2        0               0            0          0           0               0                2            Complete


<Press 1>
  Result       Host               Number       Changed        Task                          Task action             Duration
0│Ok           localhost               0       False          Gathering Facts               gather_facts                  1s
1│Ok           localhost               1       False          Print hello                   debug                         0s

<notice the playbook diredtory isn't quite the same as your workstation>


Play name: localhost:1
Task name: Print hello
Ok: localhost ['HI - HELLO..!']                                                                                                       
 0│---
 1│duration: 0.053761
 2│end: '2022-10-20T21:04:24.325620'
 3│event_loop: null
 4│host: localhost
 5│play: localhost
 6│play_pattern: localhost
 7│playbook: /root/execution-environments/hello.yml
 8│remote_addr: 127.0.0.1
 9│res:
10│  _ansible_no_log: null
11│  _ansible_verbose_always: true
12│  changed: false
13│  msg:
14│  - HI - HELLO..!
15│resolved_action: ansible.builtin.debug
16│start: '2022-10-20T21:04:24.271859'
17│task: Print hello
18│task_action: debug
19│task_args: ''
20│task_path: /root/execution-environments/hello.yml:6


<press escape to return>
```

If the image is successful at running the playbook that is a good indicator
the execution image is ready to be pushed using the same tag <TODO: IMAGE TAG BEST PRACTICES..??>

### Tag image with repo address and tag name..
```console
[root@workstation1]# podman tag <imageid> address-of-pah-or-registry/custom-python-ee:test


```

### Push the image
```console
[root@workstation1]# podman push address-of-pah-or-registry/custom-python-ee:test

Getting image source signatures
Copying blob 08f523906246 done  
Copying blob b3e2edbf73a2 done  
Copying blob fe94cd34d5ff done  
Copying blob a138a8676cac done  
Copying blob 5460c158b4fb done  
Copying blob aeead5dce8ad done  
Copying blob 59aee4c9862b done  
Copying blob 61695ea66c98 done  
Copying blob 8b15f92237e3 done  
Copying blob f99ba27272bc done  
Copying blob b3e8edec220e done  
Copying blob 5f70bf18a086 done  
Copying blob bd9ddc54bea9 skipped: already exists  
Copying blob bd9ddc54bea9 skipped: already exists  
Copying config 23176a6d37 done  
Writing manifest to image destination
Storing signatures


```


### Install 


---
## Resources

- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/assembly_installing-and-using-python_configuring-basic-system-settings -->



## Build Notes
