# VVM - Vagrant Virtual Machines

This repo provides linux virtual machines for development.  Host machine can be
your desktop (linux, mac or windows) or a bare-metal server in the datacenter.

To get started, install Git, Vagrant and VirtualBox on your host.  Then to
create and provision a new development environment:

    git clone https://github.com/andyl/VVM
    cd VVM/raw_base
    vagrant up
    vagrant ssh 

This system relies on a collection of [Ansible
Roles][1] for system provisioning and software
installation.

## Goals

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

To really understand how everything fits together, get familiar with the
[Ansible Roles](https://github.com/andyl/x-ansible) repo, the playbooks under
the `ANSIBLE` directory, and the roles for installing software components.

The Ansible provisioner will create a read-only copy of the `ANSIBLE` directory
in your working directory.  (the same directory that holds the `Vagrantfile`)
The `ANSIBLE` directory contains the ansible configuration file and a default
playbook.  This directory is re-written every time the provisioner runs.  

If you want to customize the Ansible provisioning process, copy the `ANSIBLE`
directory to another directory (like `ansible`), then edit the `Vagrantfile` to
change the `provisioning_path`.

## Packaged Machines

We publish a few standard machine configurations on our [Vagrant Cloud]:

- RAW MACHINES start with blank machine images and are provisioned dynamically
  at boot time.  Provisioning takes from 1 to 30 minutes depending on
  configuration.
- PACKAGED MACHINES are pre-provisioned - just download and run.

Six Machines in total:
- raw & packaged base - a blank machine image
- raw & packaged docker - contains everything needed to run containers
- raw & packaged exchange - total package: DB, App, Grafana, etc. etc.

Find our packaged machines on the [Vagrant Cloud](https://app.vagrantup.com/bugmark)

## How to prepare a new Packaged Machine

Run these commands:

    # get the name of the base-box you want to package
    vboxmanage list vms
    vagrant ssh -c util/x-ansible/bin/boxprep
    vagrant package --base <YOURBOX>

Your new base box will be written to `package.box`.  You can use this new base
box like any other Vagrant box.  You can share your base box publically on
Vagrant Cloud, or keep it local for private use.

## Support

Contact Andy if you have questions or would like a hand getting up to speed.  

Here are some things we can try together:
- custom base boxes for streamlined provisioning
- VM customzation for performance
- mounting source directories to use GUI editors on host machine
- development & test consoles
- moving a VM from your desktop to the datacenter
- pair-programming from your desktop VM across the firewall

When you find issues with VM configurations, port settings, network configuration, etc. please file an issue in our [Issue Tracker](https://github.com/andyl/VVM/issues).

## Appendix A: Working with Docker

TBD

## Appendix B: Working with Bare-Metal Hosts (BMH)

Sometimes we'd like to put prototype webapps on a VM for public access.  To do
this, we'll use a bare-metal host in a datacenter.  It will take a bit of
configuration to get this working smoothly.

Once this is working, we should be in good shape.  We can get a bare-metal
server with 8 cores and 96GB ram for ~$50 month.  With this resource, we could
scale a VM resource up or down as needed for demos and experiments.

### Configuring the Bare-Metal Host

The bare-metal host should run Ubuntu 18.04 and be provisioned with: `git`,
`docker`, `vagrant`, `nginx` and `docker`.  

The bare-metal host reqires a fixed IP address.

Setup a DNS entry to link your root-doman with your IP address, and add a
wildcard subdomain entry that points to the same IP.

### Setting up the VM ON the BMH

Steps:
- create server
- add IP to /etc/hosts

### SSH Access

Use SSH Jump Host

    ssh -J <metalhost> <vm>

Todo:

- Setup a 'jump' user on the metalhost with a shared passwd
  `ssh -J jump@metalhost.com <vm>`

### Nginx Proxy

- Phase 1: HostURL with port number (should work now)
- Phase 2: Use simple NGINX reverse proxy, Docker services in a VM (should work now)
- Phase 3: Modify nginx-proxy to route from subdomain (future)

The modified nginx-proxy could route to:

- a port on localhost
- an IP address and port

### Other TODOs

- backups 
- autostart VMs

## Appendix C: Resources

- [Vagrant](http://vagrantup.com)
- [VirtualBox](https://www.virtualbox.org/)
- [Machine Images](https://app.vagrantup.com/bugmark)
- [Vagrantfile Repo](https://github.com/andyl/VVM)
- [Ansible Roles](https://github.com/andyl/x-ansible)
- [Issue Tracker](https://github.com/andyl/VVM/issues) 

[1]: https://github.com/andyl/x-ansible 
[2]: https://

