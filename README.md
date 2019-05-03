# ansible_role_lab_tripleo

An Ansible role to help setup the virtual infrastructure for a small TripleO lab. This lab has x1 Undercloud, x1 Controller, and x1 Compute node. This creates the required QCOW2 storage disks for the Overcloud nodes, sets up the provisioning network, and creates an optimized Undercloud virtual machine in Libvirt.

## Requirements

None.

## Role Variables

* lab_tripleo_cpu_passthrough (boolean) = If CPU host-passthrough should be enabled for nested virtualization.
* lab_tripleo_no_log (boolean) = If sensitive tasks, such as any that use a password, should display output.
* lab_tripleo_preprovisioned (boolean) = If the deployment should use pre-provisioned/deployed nodes instead of having TripleO deploy the Overcloud nodes using Ironic.
* lab_tripleo_preprovisioned_image (string) = The master operating system image to use for all of the TripleO nodes.
* lab_tripleo_libvirt_images_location = The location of where the QCOW2 images will be stored.
* lab_tripleo_openstack_release = The OpenStack release name to use. Currently this is only used for prefixing the QCOW2 image names.
* lab_tripleo_undercloud_name = The name of the Undercloud virtual machine.
* lib_tripleo_vm_disks = A dictionary describing the name and size (in GB) of each QCOW2 image that are created.

## Role Tags

* lab_tripleo_images = The QCOW2 image creation task.
* lab_tripleo_networks = Libvirt network creation tasks.
* lab_tripleo_undercloud = Libvirt creation of the Undercloud node.
* lab_tripleo_overcloud = Libvirt creation of Overcloud nodes.

## Dependencies

None.

## Example Playbook

* Create a lab from scratch. Use Kickstart to install Enterprise Linux (EL) on the Undercloud only.

```
---
- hosts: hypervisor
  roles:
    - ansible_role_lab_tripleo
```

* Define virtual machines in Libvirt based on existing EL virtual machine images.

```
---
- hosts: hypervisor
  roles:
    - name: ansible_role_lab_tripleo
      vars:
        lab_tripleo_preprovisioned: True
        lab_tripleo_preprovisioned_image: rhel-server-7.6-x86_64-kvm.qcow2
        lab_tripleo_openstack_release: rhosp13
```

## License

Apache v2.0
