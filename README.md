# Summary

dracut-ceph-playground is an ansible scripts to test rootfs on rbd.

# Prerequires

## libvirt

Set up libvirt, allow user to access it. libvirtd.conf:

```
unix_sock_rw_perms = "0770"
unix_sock_group = "wheel"
```

https://libvirt.org/auth.html#ACL_server_config

This is needed to access libvirt without sudo from needed user.

Example virsh invocation for manual things:

```
virsh -c qemu:///system COMMAND
```

## vagrant

install vagrant and vagrant-libvirt:
https://github.com/vagrant-libvirt/vagrant-libvirt#installation


# Prepare deployment

## Install required roles

```
ansible-galaxy install -r requirements.yml
```

## Adjust variables

* vagrant_variables.yml
* site.yml

# Deploy

```
vagrant up
```

## Provision only dracut VM separately

```
ansible-playbook -i hosts dracut-playground.yml
```

# Booting from RBD

To boot from rbd, you should connect to the VM console to see the grub screen. Then choose appropriate menu option.

During first boot relabeling of SELinux context will happen and another reboot will be performed automatcally. Yuo'll be
needed to choose grub menu option again.

## Debugging

You may with to debug initramfs.
* `rd.break` lets you to be dropped into a shell right after initramfs execution
* `rd.debug` will produce lot's of shell output
