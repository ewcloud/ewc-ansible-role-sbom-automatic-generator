#  SBOM Automatic Generator Ansible Role

This repository contains a configuration template
(i.e. an [Ansible Role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html))
to customize your environment in the
[European Weather Cloud (EWC)](https://europeanweather.cloud/).

The template is designed to:

* Configure pre-existing virtual machines such that they:

  * Installs the Trivy package
  * Runs a full scan of the file system and summarizes the installed packages/version into a file with standard SPDX format

The resulting report is kept in your target host at `/sbom.json`.


## Copyright and License
Copyright © EUMETSAT 2026.

The provided code and instructions are licensed under the [MIT license](./LICENSE).
files in this repository.
They are intended to automate the setup of an environment that includes 
third-party software components.
The usage and distribution terms of the resulting environment are 
subject to the individual licenses of those third-party libraries.

Users are responsible for reviewing and complying with the licenses of
all third-party components included in the environment.

Contact [EUMETSAT](http://www.eumetsat.int) for details on the usage and distribution terms.


## Usage

The steps below assume your local file system follows the example structure below, with `ewc-ansible-role-sbom-automatic-generator` being a clone of this repository:

```text
.
├── roles
│   └── ewc-ansible-role-sbom-automatic-generator
├── inventory.yml
└── playbook.yml
```

### 1. Specify the target host and SSH credentials

Create an inventory file to specify the address and credentials that Ansible should use to reach the virtual machine you wish to configure:

```yaml
# inventory.yml
---
ewcloud:
  hosts:
    target_host:
      ansible_python_interpreter: /usr/bin/python3
      ansible_host: <add the IPv4 address of the target host>
      ansible_ssh_private_key_file: <add the path to local SSH RSA private key file>
      ansible_user: <add the username which owns the SSH RSA private key>
```


### 2. Customize the template

Create an Ansible Playbook file to load your customizations:


```yaml
# playbook.yml
---
- name: Install Trivy and generate SBOM
  hosts: all
  become: true
  become_user: root
  become_method: ansible.builtin.sudo

  roles:
    - ewc-ansible-role-sbom-automatic-generator
```

### 3. Apply the template

You can apply changes on the target host by running:

```bash
ansible-playbook -i inventory.yml playbook.yml
```

## SW Bill of Materials (SBoM)

Third-party components used in the resulting environment.

The following components will be included in the resulting environment:

| Component | Version | License | Home URL |
|------|---------|---------|--------------|
| trivy | 0.69.3 | Apache-2.0 | https://github.com/aquasecurity/trivy |

## Changelog
All notable changes (i.e. fixes, features and breaking changes) are documented 
in the [CHANGELOG.md](./CHANGELOG.md).


## Contributing

Thanks for taking the time to join our community and start contributing!
Please make sure to:
* Familiarize yourself with our [Code of Conduct](./CODE_OF_CONDUCT.md) before 
contributing.
* See [CONTRIBUTING.md](./CONTRIBUTING.md) for instructions on how to request 
or submit changes.

## Authors

[European Weather Cloud](http://support.europeanweather.cloud/) 
<[support@europeanweather.cloud](mailto:support@europeanweather.cloud)>
