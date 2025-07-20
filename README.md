# 🖥️ Ansible Libvirt VM Provisioning

This playbook automates the creation of virtual machines on a **local libvirt/KVM host** using Ansible. It is configured to run **locally** on the same system where libvirt is installed and running.

---

## 📂 Overview

* Creates VMs in `/home/user/vms`
* Uses a bootable ISO (e.g. RHCOS or RHEL) to initialize the VMs
* Generates a `libvirt` XML domain definition
* Defines and starts the VMs using `virsh`

---

## 💪 Steps to Use

### 1. Clone the Repository

```bash
git clone https://github.com/kaybee-singh/ansible-playbook-libvirt-vm
cd ansible-playbook-libvirt-vm
```

### 2. Customize the Variables

Ensure the following are in place:

* Your VM disk and ISO files are located in `/home/user/vms`
* You have a valid RHCOS or RHEL ISO, e.g. `rhcos-live.x86_64.iso`
* Edit the XML template (`testvm.xml` or similar) to set:

  * VM name
  * Memory and vCPU
  * Disk and ISO paths

#### Sample Snippet to Modify:

```xml
<domain type='kvm'>
  <name>testvm</name>
  <memory unit='MiB'>4096</memory>         <!-- Set VM Memory -->
  <vcpu>2</vcpu>                           <!-- Set vCPUs -->

  <devices>
    <disk type='file' device='disk'>
      <source file='/home/user/vms/testvm.qcow2'/>        <!-- VM Disk Path -->
      <target dev='vda' bus='virtio'/>
    </disk>
    <disk type='file' device='cdrom'>
      <source file='/home/user/vms/rhcos-live.x86_64.iso'/>  <!-- ISO Path -->
      <target dev='hdb' bus='ide'/>
      <readonly/>
    </disk>
  </devices>
</domain>
```

---

### 3. Review Playbook Tasks

This playbook does the following:

* ✅ Creates a `qcow2` disk using `qemu-img`
* ✅ Generates a domain XML from xml file.
* ✅ Defines the VM with `virsh define`
* ✅ Starts the VM
### 3. Make sure your user has rights to create the libvirt VMs.
```bash
sudo usermod -aG libvirt $(whoami)
newgrp libvirt
```
### 4. Run the Playbook
```bash

ansible-playbook create-vm-playbook.yaml
```
