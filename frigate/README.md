### LXC Setup
Create a LXC with Docker installed.
```bash
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/ct/docker.sh)"
```

Things to consider while following setup process:
- Debian
- Privileged
- 4GB disk is enough
- CPU 2 cores
- 8GB ram
- Portainer install can be ignored

#### Mount NFS directory to the LXC
```bash
# from proxmox shell
pct set <RESOURCE_ID> -mp0 /onetb/frigate,mp=/frigate
```

#### Allow hw-acceleration
Open the file from proxmox shell `/etc/pve/<RESOURCE_ID>.conf` and add the following lines.
```bash
lxc.cgroup2.devices.allow: c 226:128 rwm
lxc.mount.entry: /dev/dri/renderD128 dev/dri/renderD128 none bind,optional,create=file
features: fuse=1,nesting=1 # if not already set
```

#### USB passthrough
Open the file from proxmox shell `/etc/pve/<RESOURCE_ID>.conf` and add the following lines.
```
lxc.mount.entry: /dev/bus/usb/002/ dev/bus/usb/002/ none bind,optional,create=dir
```

#### Frigate config file
Create a file named `config.yml` in the following path `CONFIG_LOCATION/config.yml`. Check the `.env` file for `CONFIG_LOCATION`.
