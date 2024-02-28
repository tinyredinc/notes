# KVM
KVM, an acronym for Kernel-based Virtual Machine is an opensource virtualization technology integrated into the Linux kernel. It’s a type 1 (bare metal ) hypervisor that enables the kernel to act as a bare-metal hypervisor.

## KVM for UBUNTU 22.04

### Preparation
```
# Update & upgrade your system(optional)
sudo apt update
sudo apt upgrade


# Check Virtualization
egrep -c '(vmx|svm)' /proc/cpuinfo

*Your system needs to either have a VT-x( vmx ) Intel processor or an AMD-V (svm) processor. If the output is greater than 0, then virtualization is enabled. Otherwise, virtualization is disabled and you need to enable it in bios.


# Check CPU compatibility
sudo apt install -y cpu-checker

red@redhs88:~$ kvm-ok
INFO: /dev/kvm exists
KVM acceleration can be used
```
### Installation
```
# Install KVM and utilities

sudo apt install -y qemu-kvm virt-manager libvirt-daemon-system virtinst libvirt-clients bridge-utils
```
- qemu-kvm, an opensource emulator and virtualization package that provides hardware emulation.
- virt-manager, a Qt-based graphical interface for managing virtual machines via the libvirt daemon.
- libvirt-daemon-system, a package that provides configuration files required to run the libvirt daemon.
- virtinst, a set of command-line utilities for provisioning and modifying virtual machines.
- libvirt-clients,a set of client-side libraries and APIs for managing and controlling virtual machines & hypervisors from the command line.
- bridge-utils, a set of tools for creating and managing bridge devices.

```
# Enable and start the Libvirt daemon
sudo systemctl enable --now libvirtd
sudo systemctl start libvirtd
sudo systemctl status libvirtd
```
