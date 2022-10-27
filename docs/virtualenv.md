
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


### Deactivate Virtual Environment

```console
[root@workstation1]# deactivate
(venv) [root@workstation1]#

```
