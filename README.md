# Linux Server Administration Lab

Hands-on Linux system administration and networking lab built with **Ubuntu Server 26.04.1 LTS** running in **Oracle VirtualBox**. This repository documents practical tasks, verification commands, troubleshooting, and results as I develop IT support and infrastructure skills.

## Lab environment

| Component | Configuration |
| --- | --- |
| Host OS | Windows 11 |
| Virtualization | Oracle VirtualBox |
| Guest OS | Ubuntu Server LTS |
| Networking | NAT with localhost-only SSH port forwarding |
| Remote administration | OpenSSH via Windows PowerShell |
| Storage | LVM with ext4 filesystem |

## Completed work

- [x] Create and clone a GitHub repository; use Git commits and pushes
- [x] Create and configure an Ubuntu Server virtual machine
- [x] Practice terminal navigation, file creation, and Linux permissions (`chmod 600`)
- [x] Install and enable the OpenSSH server; verify its service status
- [x] Configure NAT port forwarding and connect from Windows PowerShell using SSH
- [x] Inspect memory, storage, network interfaces, and services
- [x] Update installed packages using APT
- [x] Expand an ext4 root filesystem online using LVM

## Documented lab exercises

### LVM storage management

Expanded the Ubuntu root logical volume from **13.47 GiB** to **<26.95 GiB**, using free space within the existing volume group. Verified that the mounted root filesystem grew from **14G to 27G** and its reported usage dropped from **45% to 22%**.

**[Read the LVM storage management walkthrough](docs/storage-management.md)**

## Next steps

- [ ] Learn SSH key-based authentication and harden SSH access
- [ ] Install Docker Engine and understand images, containers, and volumes
- [ ] Run an Nginx web server in a Docker container
- [ ] Explore firewall rules and Linux service administration
- [ ] Document each completed lab with commands and evidence

## Useful commands

```bash
whoami
pwd
ls -la
ip -br addr
systemctl is-active ssh
free -h
df -h /
sudo pvs
sudo vgs
sudo lvs
```

## Notes

This project is a learning environment, not a production deployment. Network configuration is described at a high level without publishing lab IP addresses or specific port mappings. Planned topics are listed separately from tasks actually completed.
