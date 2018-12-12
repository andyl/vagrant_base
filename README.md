# VVM - Vagrant Virtual Machines

This repo provides linux virtual machines for development.

Host machine can be your desktop (linux, mac or windows) or a bare-metal server
in the datacenter.

To get started, install Git, Vagrant and VirtualBox on your host.

Then to setup a new development environment:

    git clone https://github.com/andyl/VVM
    cd VVM/base
    vagrant up
    vagrant ssh 

Your VM comes pre-loaded with language runtimes (Ruby, NodeJS, Python), 
editors (vim), databases (SqLite, Postgres, Redis), etc.

Virtual machines give flexibility:
- flexible filesharing, port-forwarding, DNS & network configuration
- simple to clone/copy/backup your whole machine
- custom base-boxes for faster provisioning
- you can move your VM between desktop and datacenter
- you can run many independent VMs on a production host

This tooling is optimized for research and development.

For high-volume production, it will be better to use a more
performance-oriented virtualization (KVM/Firecracker) or containerization.

## Ansible Customization

The Ansible provisioner will create a directory `ANSIBLE` in your working
directory.  (the same directory that holds the `Vagrantfile`)  The `ANSIBLE`
directory contains the ansible configuration file and a default playbook.  This
is a read-only directory that will be re-written every time the provisioner
runs.  

If you want to customize the Ansible provisioning process, copy the `ANSIBLE`
directory to another directory (like `ansible`), then edit the `Vagrantfile` to
change the target directory.

## Support

Contact Andy if you have questions or need a hand getting up to speed.  

## Resources

- [Vagrant](http://vagrantup.com)
- [VirtualBox](https://www.virtualbox.org/)
- [Machine Images](https://app.vagrantup.com/bugmark)
- [Vagrantfile Repo](https://github.com/andyl/VVM)

