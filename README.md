# This playbook can be used to create a VM on libvirt with Ansible.

## Steps


1. Create xml file to create the VM.
2. Create Ansible playbook with following tasks
- Create the qcow2 VM disk
- Craete the VM xml file. This xml file will have info about the qcow2 that your 1st task is going to create.
- In the XML task you also have to specify the path of your Storage Pool for VMs.
- Also specify the path of your ISO of RHCOS/RHEL image.
3. Clone this repository.
```bash
git clone https://github.com/kaybee-singh/ansible-playbook-libvirt-vm
```
4. Edit the testvm.xml file and change the main values, following are main values which you can change. Feel free to tweak the file as per your requirement.
```bash
<domain type='kvm'>
  <name>testvm</name>
  <memory unit='MiB'>4096</memory>            >>>> Memory
  <vcpu>2</vcpu>                              >>> vcpu
  <os>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/home/user/vms/testvm.qcow2'/>          >>> Where your VM disk is created.
      <target dev='vda' bus='virtio'/>
    </disk>
    <disk type='file' device='cdrom'>
      <driver name='qemu' type='raw'/>
      <source file='/home/user/vms/rhcos-live-iso.x86_64.iso'/>                      >>> Where your RHCOS ISO is situated
      <target dev='hdb' bus='ide'/>
      <readonly/>
    </disk>
```
5. Now run the playbook with your user if you have permissions to create the VMs, else use the root user.
```bash
sudo ansible-playbook create-vm-playbook.yaml
