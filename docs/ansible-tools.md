## Ansible Requirements
The requirements to run the ansible tools referred to in this document
require the local installation of the python packages mentioned, and 
preferably in a [`developer environment`](./developer-environment.md)

### Ansible Tools
In order to build (`ansible-builder`), run ('ansible-runner`), and 
navigate/browse (`ansible-navigator`), the following python packages
will help when working with Execution Environments.


- ansible-builder
- ansible-runner
- ansible-navigator


### Pre-reqs
It is asserted that a python virtual environment will be used to keep 
python packages isolated from other system/user installed pythong packages.

To see the documentation on Python [`virtualenv`](./virtualenv.md), click
[here](./virtualenv.md).

### Activate Virtual Environment

Activate virtualenv

    $ source venv/bin/activate

    #Give display of virtualenv CLI appearance

### Install Ansible Tools
Install the ansible tools to build, run, and navigate Execution Environment
container images.

#### Getting Started
##### STEP 1 #####

Install the Python requirements located in the file: [`builder-requirements.txt`](./files/builder-requirements.txt)


*builder-requirements.txt*

    ansible-builder
    ansible-navigator
    ansible-runner
    
##### STEP 2 #####

Install 
- pip install -r builder-requirements.txt --user 

#### STEP 2 ####
To help with managing multiple AAP clusters for separate environments, create an inventory file for the environment being deployed. An example [`inventory.ini`](./inventory/dev-inventory.ini) can be found [here.](./inventory/dev-inventory.ini)

Validate [`ansible tools`](./build-requirements.txt) were installed successfully.



