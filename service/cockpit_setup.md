# Cockpit
Cockpit is a opensource server management tool with WEB GUI.

## Cockpit for UBUNTU 22.04

### Installation
```
# Update & upgrade your system(optional)
sudo apt update
sudo apt upgrade

# Install Cockpit via apt
sudo apt install cockpit -y

# Enable Cockpit
sudo systemctl enable --now cockpit.socket

# Set user with sudo privileges(optional)
sudo usermod -aG sudo {USERNAME}

# Set firewall rule if enabled(optional)
sudo ufw status
sudo ufw allow 9090

# Login Cockpit
https://{IPADDRESS}:9090/
```

### Cockpit Apps

https://cockpit-project.org/applications.html

- PCP Metrics - Gather metrics data about the system and display the history graphs.
```
# Install cockpit-pcp
sudo apt install cockpit-pcp
```

- Virtual Machines - Create, run, and manage virtual machines in your browser.
```
# Check cockpit and libvirtd are running properly
sudo systemctl status cockpit
sudo systemctl status libvirtd

# Install cockpit-machines
sudo apt install cockpit-machines
```

- Podman Containers - Download, use, and manage containers in your browser. (Podman replaces Docker.)
```
# Check cockpit and podman are running properly
sudo systemctl status cockpit
sudo podman run -it hello-world

# Install cockpit-podman
sudo apt install cockpit-podman
```

### Common Error

- Error message about being offline: The software update page shows “packagekit cannot refresh cache whilst offline” on a Debian or Ubuntu system.
```
Solution
Create a placeholder file and network interface.

1. Create /etc/NetworkManager/conf.d/10-globally-managed-devices.conf with the contents:
[keyfile]
unmanaged-devices=none

1.1 If you run on Ubuntu with arm64 (e.g.: on a Raspberry Pi), install extra Linux kernel modules for networking:
sudo apt install linux-modules-extra-raspi

2. Set up a “dummy” network interface:
nmcli con add type dummy con-name fake ifname fake0 ip4 1.2.3.4/24 gw4 1.2.3.1

3. Reboot
```
```
Explanation

Ubuntu uses two different network stacks which don’t play so well together.

Cockpit’s software update page uses PackageKit, which checks NetworkManager. However, as the network interfaces are primarily managed by netplan and systemd-networkd in Ubuntu, NetworkManager reports back to PackageKit that the network is offline as a critical error message and stops further actions.

Unfortunately, this means in many Ubuntu configurations, Cockpit’s software update page might fail with a message about not being able to “refresh cache whilst offline”. Other software that relies on PackageKit, such as KDE’s Discover, can also hit this bug.

Additionally, there’s nothing Cockpit itself can do to fix this problem. It’s an “emergent bug” that happens much lower in the software stack. It’s up to Ubuntu to patch the way they’re using netplan and networkd… or, alternatively, PackageKit could provide a workaround. Unfortunately, none of these fixes have been implemented yet.

Meanwhile, to be able to update your server using Cockpit when you get this error, you have to do a workaround similar to the one suggested above.
```