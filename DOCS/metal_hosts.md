# Working with Bare-Metal Hosts (BMH)

Sometimes we'd like to put prototype webapps on a VM for public access.  To do
this, we'll use a bare-metal host in a datacenter.  It will take a bit of
configuration to get this working smoothly.

Once this is working, we should be in good shape.  We can get a bare-metal
server with 8 cores and 96GB ram for ~$50 month.  With this resource, we could
scale a VM resource up or down as needed for demos and experiments.

## Setting up the VM ON the BMH

Steps:
- create server
- add IP to /etc/hosts

## SSH Access

Use SSH Jump Host

    ssh -J <metalhost> <vm>

Todo:

- Setup a 'jump' user on the metalhost with a shared passwd
  `ssh -J jump@metalhost.com <vm>`

## Remote VirtualBox

Tools: `RemoteBox` and `vboxwebsrv`

Todo:

- VboxWebSrv should be configured to auto-boot with systemd
- Make an ansible role for RemoteBox and vboxwebsrv

## Nginx Proxy

- Phase 1: HostURL with port number (should work now)
- Phase 2: Use simple NGINX reverse proxy, Docker services in a VM (should work now)
- Phase 3: Modify nginx-proxy to route from subdomain (future)

The modified nginx-proxy could route to:

- a port on localhost
- an IP address and port

## Other TODOs

- backups 
- autostart VMs
