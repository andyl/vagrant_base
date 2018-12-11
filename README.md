# VVM

Vagrant Virtual Machines

## Virtual Machines for Research and Development

This repo provides linux virtual machines for local development.

Host machine can be your desktop (linux, mac or windows) or a bare-metal server
in the datacenter.

To get started, install Git, Vagrant and VirtualBox on your host.

Then to setup a new development environment:

    git clone https://github.com/andyl/VVM
    cd VVM/devhost
    vagrant up
    vagrant ssh 

Virtual machines give flexibility:
- simple to clone/copy/backup your whole machine
- you can move your VM between desktop and datacenter
- you can run many independent VMs on a production host

This infrastructure is good for research and development.

For high-volume production, it will be better to use a more
performance-oriented virtualization (KVM/Firecracker) or containerization.

## Resources

- [Vagrant](http://vagrantup.com)
- [VirtualBox](https://www.virtualbox.org/)
- [Customized Machine Images](https://app.vagrantup.com/bugmark)
- [Vagrantfile Repo](https://github.com/andyl/VVM)

