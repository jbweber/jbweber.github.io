---
layout: default
title: Custom Cloud Images
parent: Linux
nav_order: 1
---

I maintain a fork of the official Fedora KIWI descriptions to build custom cloud images tailored for my infrastructure needs.

**Repository:** [github.com/jbweber/fedora-kiwi-descriptions](https://github.com/jbweber/fedora-kiwi-descriptions)

## Why Custom Images?

The official Fedora cloud images are fine, but I wanted an image that works well both as a cloud VM and on bare metal nodes for provisioning with [Metal3](https://metal3.io/) and [Cluster API](https://cluster-api.sigs.k8s.io/).

## Profiles

### Cloud-Base-Generic-EXT4

A standard Fedora cloud image using EXT4 instead of btrfs.

**What you get:**

- Fedora 43 cloud image (~1.56 GB installed, ~704 MB qcow2)
- 5GB virtual disk (qcow2 format, sparse/optimized)
- UEFI boot with GRUB2
- cloud-init support
- qemu-guest-agent for VM integration
- Hardware firmware support (linux-firmware)
- CPU microcode updates (Intel/AMD)

**Disk layout:**

```text
/dev/vda1 - EFI System Partition (100MB, FAT32)
/dev/vda2 - /boot (2GB, ext4)
/dev/vda3 - / root (remaining space, ext4)
```

### Cloud-Base-K8s-EXT4

Kubernetes-ready cloud image, pre-configured for immediate cluster deployment.

**Includes everything from Cloud-Base-Generic-EXT4, plus:**

- Kubernetes 1.34: kubelet, kubeadm, kubectl
- containerd 1.34: Container runtime
- Pre-configured kernel modules (`overlay`, `br_netfilter`)
- Sysctl settings for bridge netfilter and IP forwarding
- containerd config with proper CNI plugin paths
- Ready to run `kubeadm init` after first boot

**CNI plugin paths:**

- `/usr/libexec/cni` - System-provided immutable plugins
- `/var/lib/cni/bin` - Writable location for additional CNI plugins (Flannel, Calico, Cilium)
- `/opt/cni/bin` - Compatibility symlink to `/var/lib/cni/bin`

## Building

See the [repository README](https://github.com/jbweber/fedora-kiwi-descriptions/blob/f43-custom/README-CUSTOM.md) for build instructions and testing.

## References

- [Official Fedora KIWI Descriptions](https://pagure.io/fedora-kiwi-descriptions)
- [KIWI Documentation](https://osinside.github.io/kiwi/)
