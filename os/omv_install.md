# OpenMediaVault

OpenMediaVault is a highly customizable NAS operating system built on Debian. Rather than being a deeply customized NAS OS, OpenMediaVault functions more like Debian equipped with a graphical user interface (GUI) management utility for NAS-related tasks.


## Installation

There's not much to it. Simply:

- Download the image from openmediavault.org.
- Burn it onto your USB drive.
- Boot from the USB drive.
- Then, follow the on-screen instructions...

## Post Installation
- The GUI is available through HTTP using the IP address.
- Change the default password for the GUI(admin:openmediavault).
- Create a new SSH sudo user, then disable root SSH login.
    - useradd devuser
    - passwd devuser
    - usermod -a -G users,sudo,root,openmediavault-admin devuser
- Use 'omv-firstaid' if you f**ked up network configuration.

## Plugin - Must Have
OpenMediaVault is criplled without them.

### omv-extras
This plugin enables you to download third party(community) plugins.
```
sudo wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```

### openmediavault-md
This plugin is used to create, manage, and monitor Linux MD (Multiple Device) devices.
- GUI >> System >> Plugin >> openmediavault-md

## Plugin - Nice to have
Choose plugins based on your use case.

Utility
- openmediavault-wetty: Terminal access in browser over HTTP/HTTPS.
- openmediavault-iperf3: iperf3 is a tool for performing network throughput measurements.
- openmediavault-cputemp: cpu temperature plugin for openmediavault

Virtualization
- openmediavault-kvm: You tell me what is KVM.
- openmediavault-k8s: You tell me what is Kubernetes.
- openmediavault-compose: Lightweight plugin to maintain and execute docker-compose files.
- openmediavault-podman: managing containers and images, and pods made from groups of containers.


NAS Feature
- openmediavault-downloader: Download files with URL, FTP, BitTorrent, Metalink, Youtube, etc.
- openmediavault-s3: Object storage based on MinIO, compatible with Amazon S3 API.
- openmediavault-filebrowser: Provides a file managing interface within a specified directory.
- openmediavault-ftp: ProFTPD is a powerful modular FTP/SFTP/FTPS server.
- openmediavault-webdav: Web interface to enable WebDAV and select a share for files.
- openmediavault-onedrive: This plugin is synchronizing a shared folder with OneDrive cloud storage.
- openmediavault-wireguard: extremely simple yet fast and modern VPN.

## Case Study - AP Setup

Set up a WiFi hotspot using a LAN connection (enp3s0, i226) and a WLAN interface (wlp2s0, ax200). NAT and IP forwarding can be enabled through the ap-mode.sh script to route traffic from the WLAN to the LAN, which is connected to a router for internet access. The wlp2s0 interface is configured with a static IP address of 10.10.100.100, and it offers DHCP service (ranging from 10.10.100.10 to 10.10.100.50) through dnsmasq.

- sudo vim /etc/systemd/network/10-netplan-wlp2s0.network
```
[Match]
Name=wlp2s0

[Network]
Address=10.10.100.100/24
```

- sudo apt install hostapd
- sudo vim /etc/hostapd/hostapd.conf
```
# Basic Config
interface=wlp2s0
driver=nl80211

# Wi-Fi Mode
hw_mode=g
channel=6
ieee80211n=1
country_code=CA

# Wi-Fi Network
ssid=omvhotpot
wpa_passphrase=**PASSWD**
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
```
- sudo systemctl enable hostapd
- sudo systemctl start hostapd

- sudo apt install dnsmasq
- sudo vim /etc/dnsmasq.conf
```
# Disable DNS functionality
port=0

# Listen on the specific WLAN interface for DHCP requests
interface=wlp2s0
bind-interfaces

# Define the DHCP range and lease time
dhcp-range=10.10.100.10,10.10.100.50,255.255.255.0,24h
```
- sudo systemctl enable dnsmasq
- sudo systemctl start dnsmasq

- sudo vim /home/red/ap-mode.sh
```
#!/bin/bash
# This script will enble ip forward and NAT so that wlp2s0 can work like an AP
sudo sysctl net.ipv4.ip_forward=1
sudo iptables -t nat -F POSTROUTING
sudo iptables -t nat -A POSTROUTING -o enp3s0 -j MASQUERADE
sudo iptables -t nat -L -v
```
- sudo chmod +x /home/red/ap-mode.sh
- sudo /bin/bash /home/red/ap-mode.sh
