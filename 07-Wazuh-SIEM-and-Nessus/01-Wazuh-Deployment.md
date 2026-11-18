# 01 — Wazuh SIEM Deployment and Manager Recovery

## Goal

Stand up a Wazuh deployment on Proxmox VM `205` (`prod-wazuh`, `10.10.1.52`) and use its dashboard to view endpoint activity. The installed stack was version **4.14.10**.

## Verified observations

- Wazuh dashboard loaded and listed Kali/Ubuntu agents after troubleshooting.
- When the API connection failed, the dashboard showed an authorization-token / HTTP 500 problem even though index-pattern checks appeared healthy.
- `systemctl status wazuh-manager` reported `failed (Result: timeout)`.
- `journalctl` recorded `start operation timed out. Terminating.` while Wazuh components were still initializing.
- An override was saved at `/etc/systemd/system/wazuh-manager.service.d/override.conf` with a five-minute startup timeout; the restart command returned to the shell. Follow-up dashboard pages were usable, but a post-restart `systemctl is-active` screenshot was not captured for this documentation set.

## Manager troubleshooting

Review service health first:

```bash
sudo systemctl status wazuh-manager --no-pager
sudo journalctl -u wazuh-manager -n 50 --no-pager
```

Override created for the lab:

```ini
[Service]
TimeoutStartSec=300
```

Applied using:

```bash
sudo systemctl daemon-reload
sudo systemctl restart wazuh-manager
```

**Interpretation:** Increasing the timeout gives slow initialization more time; it does not fix every possible failure condition. Changes to API users/passwords during earlier troubleshooting are deliberately excluded from documentation. Credentials should be rotated and never committed.

## Evidence

- [Dashboard overview](screenshots/wazuh/01-wazuh-overview.png)
- [API connection error](screenshots/pfsense/04-wazuh-api-error.png)
- [Manager timeout](screenshots/pfsense/05-wazuh-manager-timeout.png)
- [Systemd journal timeout](screenshots/pfsense/06-manager-timeout-journal.png)
- [Systemd override editor](screenshots/pfsense/07-systemd-override-editor.png)
- [Restart after saving override](screenshots/pfsense/08-wazuh-manager-restart.png)

## Follow-up

- [ ] Confirm `sudo systemctl is-active wazuh-manager` returns `active` after a cold boot.
- [ ] Confirm Wazuh dashboard/API remains responsive after reboot.
- [ ] Rotate installation/API credentials and remove any defaults before sharing the environment.
