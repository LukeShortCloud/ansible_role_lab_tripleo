# ansible_role_lab_tripleo

An Ansible role to help setup the virtual infrastructure for a small TripleO lab. This lab has x1 Undercloud, x1 Controller, and x1 Compute node. This creates the required QCOW2 storage disks for the Overcloud nodes, sets up the provisioning network, and creates an optimized Undercloud virtual machine in Libvirt.

## Requirements

None.

## Role Variables

* lab_tripleo_libvirt_images_location = The location of where the QCOW2 images will be stored.
* lab_tripleo_undercloud_name = The name of the Undercloud virtual machine.
* lib_tripleo_vm_disks = A dictionary describing the name and size of each QCOW2 image that are created.

## Dependencies

None.

## Example Playbook

```
---
- hosts: hypervisor
  roles:
    - ansible_role_lab_tripleo
```

## License

Apache v2.0
