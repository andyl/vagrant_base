# VVM - Vagrant Virtual Machines

This repo provides linux virtual machines for development.

Host machine can be your desktop (linux, mac or windows) or a bare-metal server
in the datacenter.

To get started, install Git, Vagrant and VirtualBox on your host.

Then to create and provision a new development environment:

    git clone https://github.com/andyl/VVM
    cd VVM/base
    vagrant up
    vagrant ssh 

Virtual machines give flexibility:
- flexible filesharing, port-forwarding, DNS & network configuration
- simple to clone/copy/backup your whole machine
- you can move your VM between desktop and datacenter
- you can run many independent VMs on a production host

Your VM can be pre-loaded with language runtimes (Ruby, NodeJS, Python), 
editors (vim), databases (SqLite, Postgres, Redis), etc.

This tooling is optimized for research and development.  For high-volume
production, use performance-oriented tools like KVM / Firecracker / Docker.

## Ansible Customization

The Ansible provisioner will create a directory `ANSIBLE` in your working
directory.  (the same directory that holds the `Vagrantfile`)  The `ANSIBLE`
directory contains the ansible configuration file and a default playbook.  This
is a read-only directory that is re-written every time the provisioner runs.  

If you want to customize the Ansible provisioning process, copy the `ANSIBLE`
directory to another directory (like `ansible`), then edit the `Vagrantfile` to
change the `provisioning_path`.

## Support

Contact Andy if you have questions or would like a hand getting up to speed.  

Here are some things we can try together:
- custom base boxes for streamlined provisioning
- mounting source directories to use GUI editors on host machine
- development & test consoles
- moving a VM from your desktop to the datacenter
- pair-programming from your desktop VM across the firewall

## Resources

- [Vagrant](http://vagrantup.com)
- [VirtualBox](https://www.virtualbox.org/)
- [Machine Images](https://app.vagrantup.com/bugmark)
- [Vagrantfile Repo](https://github.com/andyl/VVM)

