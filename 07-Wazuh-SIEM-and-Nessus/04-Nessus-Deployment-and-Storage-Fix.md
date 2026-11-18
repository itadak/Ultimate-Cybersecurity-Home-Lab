# 04 — Nessus Essentials Installation, Plugin Recovery and LVM Expansion

## Goal

Deploy Nessus Essentials as the vulnerability scanner on Proxmox VM `206` (`prod-nessus`), using Ubuntu Server at `10.10.1.53/24` and gateway `10.10.1.254`. Access the interface at `https://10.10.1.53:8834` from Kali and prepare to scan a **lab-owned** target.

## Setup observed

- Nessus **10.12.7** installed, launched and accessible in its browser interface.
- Nessus Essentials activation completed. The screenshot with the **activation code** is excluded from this public-ready package.
- The license utilization page showed 0 of 5 IPs used at the time of capture.
- The scanner repeatedly displayed `Plugins are compiling`, so scan creation was unavailable during this phase.

## Plugin update and load failure

Log evidence showed:

```text
Finished plugin update
Plugin load failed: Attempting to rebuild plugins
Plugins rebuilt successfully
```

The successful rebuild was encouraging, but the user interface continued to report compilation afterward. We checked disk space rather than assuming CPU activity alone meant progress.

## Root cause: full root filesystem

`df -h /` showed the root filesystem at **100% capacity**:

```text
Filesystem                              Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv      19G   18G     0  100% /
```

Proxmox already assigned VM 206 a **40 GB disk**. Ubuntu's `lsblk` showed a `38G` LVM partition, but the root logical volume was only `19G`. `sudo vgs` showed **19G free** inside volume group `ubuntu-vg`.

This was not primarily a small Proxmox disk: the virtual disk capacity existed but had **not been assigned to the root logical volume**.

## Repair performed

After confirming 19G of unused LVM extents, the root logical volume and ext4 filesystem were extended **online**:

```bash
sudo vgs
sudo lvextend -l +100%FREE -r /dev/ubuntu-vg/ubuntu-lv
df -h /
```

The command completed successfully without deleting files or increasing the Proxmox virtual disk size.

**Verified after expansion:**

| Root volume metric | Before | After |
|---|---:|---:|
| Capacity | 19 GB | 38 GB |
| Available | 0 GB | 19 GB |
| Usage | 100% | 47% |

A later Nessus screenshot showed **no plugin compilation banner** and the **New Scan** action displayed. **A completed scan or vulnerability report has not yet been provided**, so they are not claimed here.

## Evidence

- [VM network setup](screenshots/nessus/01-nessus-vm-network.png)
- [Initial Nessus web setup](screenshots/nessus/02-nessus-initial-web-setup.png)
- [Plugins compiling](screenshots/nessus/03-plugin-compiling-notice.png)
- [Plugin feed update](screenshots/nessus/04-plugin-feed-update.png)
- [Plugin load failure](screenshots/nessus/05-plugin-load-failed.png)
- [Plugin rebuild succeeded](screenshots/nessus/06-plugin-rebuilt-successfully.png)
- [Full root filesystem](screenshots/nessus/07-root-filesystem-full.png)
- [40 GB disk in Proxmox](screenshots/nessus/08-proxmox-disk-40gb.png)
- [19 GB root LV](screenshots/nessus/09-lvm-volume-19gb.png)
- [19 GB unallocated LVM extents](screenshots/nessus/10-lvm-free-space-19gb.png)
- [Successful online disk expansion](screenshots/nessus/11-lvm-online-expansion-success.png)
- [Scan creation interface after repair](screenshots/nessus/12-nessus-new-scan-screen.png)

## Next test (not yet completed)

- [ ] Verify Nessus scanner is **Ready** and scan templates are accessible.
- [ ] First test should target **only** Metasploitable2 (`10.10.10.50`) or another explicit lab-owned target.
- [ ] Confirm pfSense rules permit only the intended testing path and that vulnerable VMs are not exposed to external networks.
- [ ] Run a scoped, non-destructive Basic Network Scan; save the scan report and document findings accurately.
- [ ] Monitor disk consumption and set capacity alerts before plugin data fills the filesystem again.
