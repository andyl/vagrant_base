# VVM - Vagrant Virtual Machines

This repo provides linux virtual machines for development.  Host machine can be
your desktop (linux, mac or windows) or a bare-metal server in the datacenter.

To get started, install [Git][git], [Vagrant][vgr] and [VirtualBox 5.2][box] on
your host.  Then to create a new virtual machine:

    git clone https://github.com/andyl/VVM
    cd VVM/packaged_full
    vagrant up
    vagrant ssh 

The first time you run the `vagrant up` command, a large (~2.5GB) machine image
will be downloaded.  After that, creating new machines from this image takes a
minute or two.

## Goals

Fast developer onboarding and operational flexibility:
- flexible filesharing, port-forwarding, DNS & network configuration
- simple to clone/copy/backup your whole machine
- you can move your VM between desktop and datacenter
- you can run many independent VMs on a production host

Our VM images are pre-loaded with language runtimes (Ruby, NodeJS, Python),
editors (vim), databases (SqLite, Postgres, Redis), etc.  The machine images
are extensible and customizable.

This tooling is optimized for research and software development.  For
production, use performance-oriented tools like KVM / Firecracker / Docker.

## Ansible for Provisioning

Machine provisioning is done with [Ansible][ans].  Study the [Ansible
Roles][anx] repo, the playbooks under the `ANSIBLE` directory, and the roles
for installing software components.

The Ansible provisioner will create a read-only copy of the `ANSIBLE` directory
in your working directory.  (the same directory that holds the `Vagrantfile`)
The `ANSIBLE` directory contains the ansible configuration file and a default
playbook.  This directory is re-written every time the provisioner runs.  

If you want to customize the Ansible provisioning process, copy the `ANSIBLE`
directory to another directory (like `ansible`), then edit the `Vagrantfile` to
change the `provisioning_path`.

## Packaged Machines

Find pre-packaged machine images on our [Vagrant Cloud][vgc]

Machine Profiles:
- `base` - the simplest base machine
- `full` - has language runtimes and editors, databases and Docker support

For each profile, we have two variants:
- RAW MACHINES start with a blank machine and are provisioned at boot time.
  Provisioning takes 5-20 mins. 
- PACKAGED MACHINES are pre-provisioned to download and run.  Machine images
  are 1-3GB.  Create new machines in about 90 seconds.

## How to create a new Packaged Machine

First create a custom-configured live VM.  Then run these commands:

    # get the name of the base-box you want to package
    vboxmanage list vms
    vagrant ssh -c util/x-ansible/bin/boxprep
    vagrant package --base <YOURBOX>

Your new base box will be written to `package.box`.  You can use this new base
box like any other Vagrant box.  You can share your base box publically on
Vagrant Cloud, or keep it local for private use.

To share on the Vagrant Cloud, visit their [website][vgc] and use their
publishing tools.

## Support

Contact Andy for support.  Here are some things we can try together:
- custom base boxes for streamlined provisioning
- VM customzation for performance
- mounting source directories to use GUI editors on host machine
- development & test consoles
- moving a VM from your desktop to the datacenter
- pair-programming from your desktop VM across the firewall

Please post bugs and questions to our [Issue Tracker][vvt].

## Appendix A: Working with Docker

We like Docker for service deployment, and publish a suite of reusable
docker services at [Casmacc.io][csm].

Do a quick test on a Docker-enabled host:

    TERMINAL1> docker run -p 3060:80   casmacc/html_helloworld
    TERMINAL2> docker run -p 3061:3090 casmacc/sinatra_helloworld
    TERMINAL3> docker run -p 3062:4000 casmacc/phoenix_helloworld
    TERMINAL4> curl localhost:3060
    TERMINAL4> curl localhost:3061
    TERMINAL4> curl localhost:3062

## Appendix B: Working with Bare-Metal Hosts (BMH)

VMs can run in a Bare-Metal host in a datacenter.

We can get a bare-metal server with 16 cores and 96GB ram for ~$50 month.  With
this capacity, we could scale a VM up or down as needed for demos and
experiments.

Note that both Google Compute Engine and Azure support a `Nested
Virtualization` option that allows you to run Virtual Box.  The Nested
Virtualization machines run about $100/month for a machine with decent
performance.  AWS also has bare metal servers that cost in the range of $5-7
per hour.

### Configuring the Bare-Metal Host

The bare-metal host should run Ubuntu 18.04 and be provisioned with: `git`,
`docker`, `vagrant` and `virtualbox`.  The BMH reqires a fixed IP address.

Setup a DNS entry to link your root-doman with your IP address, and add a
wildcard subdomain entry that points to the same IP.

### SSH Access

Setup your VM for ssh access on a unique port.  Then:

    ssh <host> -p <VM-port>

### Nginx Proxy

Use the Casmacc [Nginx-Proxy][ngp] container to route subdomains (like
`intern.bugmark.tech`) to guest VMs.  See the [README
Notes](https://github.com/casmacc/nginx_proxy#proxy-to-a-vm-guest) for
configuration information.

## Appendix C: Binary Large Objects

We use a handful of pre-compiled executables, packages and tar files.  These
are large binary objects, not well suited for management with Git.  For now
we're simply posting the objects onto a webserver with `rsync`, and making the
objects available for download during Ansible provisioning.  The files are
currently served [here](http://files.casmacc.net).

## Appendix D: Collaboration

The `full` image is configured with tools for remote collaboration and
pair-programming.

### SSH-CHAT

Connect to the SSH-Chat server from the command line. Run `sshchat`.

Note: you can setup a project-specific ssh-chat service using [SSH-Chat Docker][scd]

[scd]: http://github.com/casmacc/ssh-chat

### TMATE Terminal Sharing

Session host:
- start a tmate session `tmate`
- publish the session address `tmate_address`
- the session address is auto-published onto SSH-Chat

Session participant:
- enter the ssh command with session address on your command line

### WORMHOLE File Transfer

Sender:
- type `wormhole send <filename>`
- note the wormhole code

Receiver:
- get the wormhole code from the sender
- type `wormhole receive <code>`

## Appendix E: Host/Guest Shared Files

The host directory is mounted on the guest at `/vagrant`.

If you want to share other directories, edit the `Vagrantfile` and follow the
[synced-folder guide][sfg].

[sfg]: https://www.vagrantup.com/docs/synced-folders/basic_usage.html

## Appendix F: Remote Shared Files

If your VM is running on a bare-metal host in a datacenter, you can use `sshfs`
to mount a VM filesystem locally.

    mkdir ./mountpoint
    sshfs [user@]<BARE-METAL-DOMAIN> ./mountpoint -p <VM-port>

## Appendix G: Collaboration on a Mac Host

You can use SSH-Chat and Tmate on your Mac host.  

This is useful if you'd like help with setting up your VM.

SSH-Chat just works, open a terminal then `$ ssh casmacc.net -p 9000`.

To install Tmate, open a terminal then `$ brew install tmate`.

To start Tmate, type `$ tmate`.  Then from within tmate, type:

    $ echo -e $(tmate show-messages) "\r" | ssh casmacc.net -p 9000

Your partner will be able to find the attachment code on sshchat and connect.

## Appendix H: Virtual Machines and File-Watchers

Oftentimes you'll use test-runners and build tools on your VM that are built on
file-watchers like `grunt` and `guard`.

Unfortunatly the guest OS will not be notified when a file is changed in a folder shared with the host.

To resolve this problem, install plugin `vagrant-notify-forwarder`.

    $ vagrant plugin install vagrant-notify-forwarder

Reload your VM after installing the plugin...

## Appendix I: Resources

- [Git][git]
- [Vagrant][vgr]
- [VirtualBox][box]
- [Machine Images][vgc]
- [Vagrantfile Repo][vvr]
- [Vagrantfile Repo Tracker][vvt]
- [Ansible][ans]
- [Ansible Roles][anx]
- [Casmacc.io][csm]
- [Nginx Proxy][ngp]

[git]: https://git-scm.com
[vvr]: https://github.com/andyl/VVM
[vvt]: https://github.com/andyl/VVM/issues
[ans]: https://www.ansible.com/
[anx]: https://github.com/andyl/x-ansible 
[vgr]: https://vagrantup.com
[vgc]: https://app.vagrantup.com/bugmark
[box]: https://www.virtualbox.org
[csm]: https://casmacc.io
[ngp]: https://github.com/casmacc/nginx_proxy
[ngi]: https://github.com/andyl/VVM/issues/5
