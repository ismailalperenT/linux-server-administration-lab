# Linux LVM Storage Management — Root Filesystem Expansion

## Overview

This lab documents a hands-on storage administration task performed on an Ubuntu Server virtual machine running in Oracle VirtualBox. The root filesystem was expanded online using free capacity already available in an LVM volume group.

**Environment**
- Operating system: Ubuntu Server 26.04.1 LTS
- Hypervisor: Oracle VirtualBox 7.2.22
- Virtual disk: 30 GB (VirtualBox VDI)
- Physical volume: `/dev/sda3`
- Volume group: `ubuntu-vg`
- Logical volume: `ubuntu-lv`
- Root filesystem: ext4, mounted at `/`
- Administration: SSH from Windows PowerShell via local NAT port forwarding

## Objective

Increase the usable root filesystem capacity without reinstalling Ubuntu or changing the size of the VirtualBox virtual disk.

> **Scope:** This was a learning lab on a disposable virtual machine, not a production server. A VM snapshot before storage changes is recommended. Do not copy storage modification commands onto a different system without verifying its actual volume and filesystem paths.

## 1. Inspect current storage

Run these commands to inspect the physical volume (PV), volume group (VG), logical volume (LV), and mounted root filesystem.

```bash
sudo pvs
sudo vgs
sudo lvs
df -h /
```

Observed values before expansion:

| Metric | Observed value |
| --- | ---: |
| Volume group total | <26.95 GiB |
| Unallocated VG space (VFree) | 13.47 GiB |
| Root LV size | 13.47 GiB |
| Root filesystem size (`df -h /`) | 14G |
| Root filesystem used | 5.6G |
| Root filesystem available | 7.0G |
| Root filesystem usage | 45% |

The virtual disk was 30 GB, but the root LV did not initially use the full capacity of the LVM volume group. The difference included space reserved for other partitions such as `/boot` and `/boot/efi`.

## 2. Expand the logical volume and ext4 filesystem

After identifying `ubuntu-vg/ubuntu-lv` and confirming that the volume group had free extents, run:

```bash
sudo lvextend -l +100%FREE -r /dev/ubuntu-vg/ubuntu-lv
```

Explanation:
- `lvextend`: increases the logical volume size.
- `-l +100%FREE`: allocates all remaining free extents in this volume group.
- `-r`: requests resizing of the filesystem along with the logical volume.
- `/dev/ubuntu-vg/ubuntu-lv`: identifies the root logical volume on this VM.

The output confirmed:
- Logical volume increased from **13.47 GiB** to **<26.95 GiB**.
- `resize2fs` expanded the mounted ext4 root filesystem online.
- `Logical volume ubuntu-vg/ubuntu-lv successfully resized.`

No reboot was required for this online ext4 expansion.

## 3. Verify the result

```bash
sudo lvs
sudo vgs
df -h /
```

Observed values after expansion:

| Metric | Before | After |
| --- | ---: | ---: |
| Root LV | 13.47 GiB | <26.95 GiB |
| Unallocated VG space | 13.47 GiB | 0 |
| Root filesystem size | 14G | 27G |
| Used | 5.6G | 5.6G |
| Available | 7.0G | 20G |
| Usage | 45% | 22% |

This confirms that the logical volume and mounted filesystem were both expanded successfully.

## 4. Lessons learned

- Understand the distinction between a **physical volume**, **volume group**, **logical volume**, and **filesystem**.
- Check `pvs`, `vgs`, `lvs`, and `df` before and after making storage changes.
- The `-l` option uses a **lowercase letter L**, not the digit `1`. A mistyped `-1` produced an invalid-option error; rerunning with `-l` succeeded.
- Increasing an LV alone is not sufficient unless the filesystem is also resized. The `-r` option handled both steps in this lab.
- Leaving free extents in a VG allows future expansion; using `+100%FREE` consumes that reserve.

## Skills demonstrated

Linux administration · LVM storage management · ext4 filesystem resizing · SSH · VirtualBox · CLI troubleshooting

---

**Evidence:** Values and outcome are transcribed from the terminal outputs captured during the lab. Screenshots may be added to this repository later.
