# Ansible Role: Spice install (LUDUS)

An Ansible Role that installs [SPICE](https://spice-space.org/) on Windows and Linux virtual machines hosted by Proxmox.

Spice allows in combination with [virt-viewer](https://virt-manager.org/download.html) to handle on the local linux physical or virtual machine Display (and Clipboard), Sound, Shares and Usb keys redirections.

To use Spice display, the role needs to reconfigure the VM display to QXL.

Optionnaly it can also add other Spice functionnalities :
- add a compatible usb controller and a package to mount a key remotely
- add a package to allow webdav shares 

After Ansible update :
- add a compatible sound card and a package to redirect the sound

> [!WARNING]
> This role changes VM display to QXL that is needed to activate SPICE on Proxmox.
> Optional features will add compatible components to make SPICE work.
> This role will need to stop and start your concerned VM.

## Identified bugs to fix and features to add

- [X] Works only domain joined VM
- [ ] Fix : make it work on standalone VMs
- [ ] Feature : add sound support ( Ansible [merge](https://github.com/ansible-collections/community.general/pull/9847) request )
- [ ] Feature : add Linux support

## Requirements

Manual installation in LUDUS.

```bash
# Installation steps
git clone https://github.com/Croko-fr/ludus_install_spice.git
ludus ansible role add -d ludus_install_spice
```

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):
```yaml
    # Add a SPICE usb device to the VM
    spice_usb: true
    # Enable SPICE network shares compatibility
    spice_share: true
```

## Dependencies

None.

## Example Playbook

```yaml
- hosts: hosts
  roles:
    - ludus_install_spice
  vars:
    spice_usb: true
    spice_share: true
```

## Example Ludus Range Config

```yaml
ludus:
  - vm_name: "{{ range_id }}-ad-dc-win2022-server-x64-1"
    hostname: "{{ range_id }}-DC01-2022"
    template: win2022-server-x64-template
    vlan: 10
    ip_last_octet: 11
    ram_gb: 6
    cpus: 4
    windows:
      sysprep: true
    domain:
      fqdn: ludus.domain
      role: primary-dc
    roles:
      - ludus_install_spice
    role_vars:
      spice_usb: true
      spice_share: true
```

## License

GPLv3

## Author Information

This role was created by [Croko-fr](https://github.com/croko-fr), for [Ludus](https://ludus.cloud/).
