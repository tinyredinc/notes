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


NAS Enhance
- openmediavault-downloader: Download files with URL, FTP, BitTorrent, Metalink, Youtube, etc.
- openmediavault-s3: Object storage based on MinIO, compatible with Amazon S3 API.
- openmediavault-filebrowser: Provides a file managing interface within a specified directory.
- openmediavault-ftp: ProFTPD is a powerful modular FTP/SFTP/FTPS server.
- openmediavault-webdav: Web interface to enable WebDAV and select a share for files.
- openmediavault-onedrive: This plugin is synchronizing a shared folder with OneDrive cloud storage.
- openmediavault-wireguard: extremely simple yet fast and modern VPN.