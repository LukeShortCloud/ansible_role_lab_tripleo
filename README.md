# ansible_role_lab_tripleo

An Ansible role to help setup the virtual infrastructure for a small TripleO lab. This lab has x1 Undercloud, x1 Controller, and x1 Compute node. This creates the required QCOW2 storage disks for the Overcloud nodes, sets up the provisioning network, and creates an optimized Undercloud virtual machine in Libvirt.

## Requirements

None.

## Role Variables

* lab_tripleo_copy_method (string) = The method to copy pre-provisioned node images. "ansible", "qemu-img", or "reflink" Default: qemu-img.
* lab_tripleo_cpu_passthrough (boolean) = If CPU host-passthrough should be enabled for nested virtualization.
* lab_tripleo_no_log (boolean) = If sensitive tasks, such as any that use a password, should display output.
* lab_tripleo_console (boolean) = If a CLI console should be setup instead of a GUI one.
* lab_tripleo_{undercloud,overcloud}_{cpus,memory} (integer) = The number of processor or RAM allocation to each type of node.
* lab_tripleo_preprovisioned (boolean) = If the deployment should use pre-provisioned/deployed nodes instead of having TripleO deploy the Overcloud nodes using Ironic.
* lab_tripleo_preprovisioned_image (string) = The master operating system image to use for all of the TripleO nodes.
* lab_tripleo_libvirt_images_location (string) = The location of where the QCOW2 images will be stored.
* lab_tripleo_openstack_release (string) = The OpenStack release name to use. Currently this is only used for prefixing the QCOW2 image names.
* lab_tripleo_undercloud_name (string) = The name of the Undercloud virtual machine.
* lab_tripleo_undercloud_node (dictionary) = A dictionary describing the name and size of the QCOW2 image that is created.
    * name (string) = The name of the Undercloud virtual machine.
    * size (integer) = The size, in GB, of the QCOW2 image.
* lab_tripleo_overcloud_nodes (dictionary) = A dictionary describing the name, MAC address of the network interface card to PXE boot on, and the size of each QCOW2 image that is created.
    * mac (string) = The MAC address to use for the PXE boot interface.
    * name (string) = The name of the virtual machine. It is recommended to name it after it's TripleO Role (Controller, Compute, etc.).
    * size (integer) = The size, in GB, of the QCOW2 image.

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
