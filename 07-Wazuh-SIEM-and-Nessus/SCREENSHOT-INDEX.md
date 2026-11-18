# Screenshot Index

**37 curated screenshots** from the user's Episode 3 setup and troubleshooting. These are separate from the larger raw screenshot archive.

Paths are relative to this documentation folder. Image timestamps are approximate capture names, not proof of external publication time.

## Wazuh, endpoint and Docker monitoring

- [01-wazuh-overview.png](screenshots/wazuh/01-wazuh-overview.png) — Wazuh dashboard became accessible during initial manager deployment.
- [02-kali-agent-manager-config.png](screenshots/wazuh/02-kali-agent-manager-config.png) — Kali agent configuration points to Wazuh Manager 10.10.1.52:1514 TCP.
- [03-ubuntu-original-wrong-address.png](screenshots/wazuh/03-ubuntu-original-wrong-address.png) — Ubuntu agent initially configured to incorrect 10.0.0.2 manager address.
- [04-docker-listener-config.png](screenshots/wazuh/04-docker-listener-config.png) — Ubuntu Wazuh agent ossec.conf docker-listener enabled.
- [05-ubuntu-agent-active.png](screenshots/wazuh/05-ubuntu-agent-active.png) — Ubuntu agent 002 active at 10.10.30.50 on Wazuh dashboard.
- [06-docker-listener-started.png](screenshots/wazuh/06-docker-listener-started.png) — Wazuh docker-listener module started and began listening for Docker events.
- [07-docker-hello-world-test.png](screenshots/wazuh/07-docker-hello-world-test.png) — Docker hello-world container ran successfully with sudo.
- [08-ubuntu-threat-hunting.png](screenshots/wazuh/08-ubuntu-threat-hunting.png) — Threat Hunting shows Ubuntu agent security alerts.
- [09-docker-search-sudo-only.png](screenshots/wazuh/09-docker-search-sudo-only.png) — Search for docker returned sudo events only; Docker alert ingestion not proven.

## pfSense agent, manager troubleshooting and firewall rules

- [01-package-search.png](screenshots/pfsense/01-package-search.png) — pfSense package search found wazuh-agent-4.14.10 while 4.7.2 was unavailable.
- [02-freebsd-version-mismatch.png](screenshots/pfsense/02-freebsd-version-mismatch.png) — FreeBSD repository version mismatch warning during proposed package install.
- [03-manager-address-config.png](screenshots/pfsense/03-manager-address-config.png) — pfSense Wazuh agent configuration targets manager 10.10.1.52.
- [04-wazuh-api-error.png](screenshots/pfsense/04-wazuh-api-error.png) — Wazuh dashboard temporary API connection failure, HTTP 500.
- [05-wazuh-manager-timeout.png](screenshots/pfsense/05-wazuh-manager-timeout.png) — Wazuh manager systemd service failed with timeout.
- [06-manager-timeout-journal.png](screenshots/pfsense/06-manager-timeout-journal.png) — journalctl confirmed service startup timeout.
- [07-systemd-override-editor.png](screenshots/pfsense/07-systemd-override-editor.png) — Editing systemd override for Wazuh Manager startup timeout.
- [08-wazuh-manager-restart.png](screenshots/pfsense/08-wazuh-manager-restart.png) — Systemd override saved and manager restart attempted.
- [09-firewall-agent-disconnected.png](screenshots/pfsense/09-firewall-agent-disconnected.png) — pfSense agent previously appeared disconnected in Wazuh (version 4.7.2).
- [10-agent-service-running.png](screenshots/pfsense/10-agent-service-running.png) — pfSense Wazuh agent components reported running.
- [11-merged-mgt-permission-denied.png](screenshots/pfsense/11-merged-mgt-permission-denied.png) — Agent log reported Permission denied opening shared merged.mgt.
- [12-shared-directory-before-fix.png](screenshots/pfsense/12-shared-directory-before-fix.png) — Shared directory owned root:wazuh with root-only write access; merged.mgt absent.
- [13-shared-directory-permission-change.png](screenshots/pfsense/13-shared-directory-permission-change.png) — Shared directory changed to drwxrwx--- root:wazuh.
- [14-agent-restart-log.png](screenshots/pfsense/14-agent-restart-log.png) — pfSense Wazuh agent restarted successfully; new log shows modules active.
- [15-shared-firewall-log-config.png](screenshots/pfsense/15-shared-firewall-log-config.png) — Wazuh centralized agent config monitors /var/log/filter.log as syslog.
- [16-custom-pfsense-rule.png](screenshots/pfsense/16-custom-pfsense-rule.png) — Wazuh custom pfSense firewall drop rule ID 87701 editor.

## Nessus deployment and disk troubleshooting

- [01-nessus-vm-network.png](screenshots/nessus/01-nessus-vm-network.png) — VM 206 network configured to 10.10.1.53/24 with gateway 10.10.1.254.
- [02-nessus-initial-web-setup.png](screenshots/nessus/02-nessus-initial-web-setup.png) — Nessus initial setup screen at https://10.10.1.53:8834.
- [03-plugin-compiling-notice.png](screenshots/nessus/03-plugin-compiling-notice.png) — Nessus Essentials dashboard reported plugins compiling.
- [04-plugin-feed-update.png](screenshots/nessus/04-plugin-feed-update.png) — Nessus logs show plugin feed update completion and service restart.
- [05-plugin-load-failed.png](screenshots/nessus/05-plugin-load-failed.png) — Nessus plugin load failed and rebuilt plugins.
- [06-plugin-rebuilt-successfully.png](screenshots/nessus/06-plugin-rebuilt-successfully.png) — Nessus logs show plugins rebuilt successfully.
- [07-root-filesystem-full.png](screenshots/nessus/07-root-filesystem-full.png) — Nessus Ubuntu root filesystem 19 GB at 100% full.
- [08-proxmox-disk-40gb.png](screenshots/nessus/08-proxmox-disk-40gb.png) — Proxmox VM 206 has 40 GB virtual disk and 4 GB RAM.
- [09-lvm-volume-19gb.png](screenshots/nessus/09-lvm-volume-19gb.png) — lsblk shows 40 GB disk, 38 GB LVM partition, 19 GB root logical volume.
- [10-lvm-free-space-19gb.png](screenshots/nessus/10-lvm-free-space-19gb.png) — vgs confirms 19 GB unallocated space in ubuntu-vg.
- [11-lvm-online-expansion-success.png](screenshots/nessus/11-lvm-online-expansion-success.png) — lvextend -r succeeded; root filesystem now 38 GB with 19 GB free.
- [12-nessus-new-scan-screen.png](screenshots/nessus/12-nessus-new-scan-screen.png) — Nessus All Scans after storage repair; compile warning absent and New Scan button visible.
