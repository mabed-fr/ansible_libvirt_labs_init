
# mabed.ansible_libvirt_labs_init

[![Ansible version](https://img.shields.io/badge/ansible-%3E%3D2.10-black.svg?style=flat-square&logo=ansible)](https://github.com/ansible/ansible)

⭐ Star us on GitHub — it motivates us a lot!

Configurer rapidement un labs libvirt(kvm)

**Platforms Supported**:

| Platform | Versions |
|----------|----------|
| Debian | 11 |
| Debian | 10 |

## ⚠️ Requirements

Ansible >= 2.1.
libvirt-python (pip install)

### Ansible role dependencies

None.

## ⚡ Installation

### Install with Ansible Galaxy

```shell
ansible-galaxy install mabed.ansible_libvirt_labs_init
```

### Install with git

If you do not want a global installation, clone it into your `roles_path`.

```bash
git clone mabed.ansible_libvirt_labs_init
```

But I often add it as a submodule in a given `playbook_dir` repository.

```bash
git submodule add roles/mabed.ansible_libvirt_labs_init
```

As the role is not managed by Ansible Galaxy, you do not have to specify the
github user account.

### ✏️ Example Playbook

Basic usage is:

```yaml
- name: KVM labs
  hosts:
    - localhost
  roles:
    - role: ansible_libvirt_labs_init
  tags:
    - ansible_libvirt_labs_init
```

## ⚙️ Role Variables

Variables are divided in three types.

The **default vars** section shows you which variables you may
override in your ansible inventory. As a matter of fact, all variables should
be defined there for explicitness, ease of documentation as well as overall
role manageability.

The **context variables** are shown in section below hint you
on how runtime context may affects role execution.

### Default variables
Role default variables from `defaults/main.yml`.


### Context variables

Those variables from `vars/*.{yml,json}` are loaded dynamically during task
runtime using the `include_vars` module.

Variables loaded from `vars/main.yml`.

| Variable Name | Value |
|---------------|-------|
| libvirt_labs_init_dest_download_images | /tmp/ |
| libvirt_labs_init_libvirt_pool_dir | /var/lib/libvirt/images/ |
| libvirt_labs_init_debian_11_wanted | True |
| libvirt_labs_init_debian_10_wanted | False |
| libvirt_labs_init_vm_global_root_pass | root |
| libvirt_labs_init_vm_ssh_key_path | /home/$USER/.ssh/id_rsa.pub |
| libvirt_labs_init_install_packages | smem,htop |

### ✏️ Example host_vars / group_vars

I want to have :
* 2 apache fronts  Debian11 --> 4vcpu
* 1 haproxy loadbalancer Debian11 --> 2vcpu
* Packages atop & locate
* DISK 10GO
* 2 Go RAM


```yaml
---
libvirt_labs_init_install_packages: "atop,locate"
libvirt_labs_init_debian_11_wanted: true

vms:
  haproxy_lb:
    name: "haproxy_lb"
    title: "haproxy_lb"
    description: "haproxy load balancer for fronts"
    root_pass: "{{ libvirt_labs_init_vm_global_root_pass }}"
    os: "Debian_11"
    vcpus: 2
    ram_mib: 1024
    net: "default"
    disk_size_gib: 10
    libvirt_pool_dir: "{{ libvirt_labs_init_libvirt_pool_dir }}"
    packages: "{{ libvirt_labs_init_install_packages }}"
  front2:
    name: "front2"
    title: "front2"
    description: "Front 2 "
    root_pass: "{{ libvirt_labs_init_vm_global_root_pass }}"
    os: "Debian_11"
    vcpus: 4
    ram_mib: 2048
    net: "default"
    disk_size_gib: 10
    libvirt_pool_dir: "{{ libvirt_labs_init_libvirt_pool_dir }}"
    packages: "{{ libvirt_labs_init_install_packages }}"
  front1:
    name: "front1"
    title: "front1"
    description: "Front 1"
    root_pass: "{{ libvirt_labs_init_vm_global_root_pass }}"
    os: "Debian_11"
    vcpus: 4
    ram_mib: 2048
    net: "default"
    disk_size_gib: 10
    libvirt_pool_dir: "{{ libvirt_labs_init_libvirt_pool_dir }}"
    packages: "{{ libvirt_labs_init_install_packages }}"
...
```

Init your lab ? Play !

```shell

ansible-playbook site.yml --limit "localhost" -D

```


Need clean UP ? Play !


```shell

ansible-playbook site.yml --limit "localhost" -D --tags retired

```

