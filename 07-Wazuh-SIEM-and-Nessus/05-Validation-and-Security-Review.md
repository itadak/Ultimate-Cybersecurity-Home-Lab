# 05 — Validation, Limitations and Security Review

## Evidence-based validation matrix

| Item | Current status | Evidence / remaining limitation |
|---|---|---|
| Wazuh dashboard | **Verified accessible** | Dashboard screenshots and Ubuntu Threat Hunting views |
| Wazuh Manager | **Recovered for dashboard use** | Earlier systemd timeout; persistent healthy status after cold boot not yet captured |
| Kali agent | **Verified Active** | Kali configuration and previous Wazuh active-agent view |
| Ubuntu agent | **Verified Active** | Agent 002 dashboard screenshot at 10.10.30.50 |
| Ubuntu security logs | **Verified ingested** | 238 hits in a captured 24-hour Threat Hunting view |
| Docker listener | **Verified started** | Agent log shows module startup and listening |
| Docker-specific security events in Wazuh | **Not verified** | Searching `docker` found sudo alerts, not confirmed container events |
| pfSense Wazuh process | **Verified running/restarted** | `service wazuh-agent` output |
| pfSense agent Active in dashboard | **Not verified after repair** | Earlier disconnected; restart success is not enough |
| pfSense firewall log ingestion | **Configuration shown, receipt unverified** | Shared config for `filter.log`; no matched events documented |
| Custom pfSense rule 87701 | **Drafted in Wazuh editor** | Save and alert match unverified |
| Nessus web UI | **Verified accessible** | Setup and All Scans screenshot |
| Nessus plugins | **Rebuild reported successful** | Logs; no banner in later screenshot |
| Nessus root disk | **Verified fixed** | Root 38G, 19G available, 47% usage |
| Nessus vulnerability scan | **Not yet verified** | No scan report or results captured |

## Security priorities before publishing or extending testing

1. **Do not publish secrets.** Wazuh installation passwords and API accounts appeared in earlier screenshots; a Nessus activation code also appeared. None of those sensitive screenshots were intentionally included here, but review every image again before making a repository public.
2. **Verify isolation.** Metasploitable2 previously reached the internet and the earlier pfSense policies appeared permissive. VLAN membership alone does not guarantee isolation. No unrestricted vulnerability scans or attack demonstrations should be run until the vulnerable target networks are restricted.
3. **Avoid incompatible pfSense packages.** The FreeBSD `IGNORE_OSVERSION` warning should not be bypassed casually; maintain a recovery backup and use supported methods where practical.
4. **Use least privilege.** Docker access via `sudo` is acceptable for an administrative test; granting general membership in the `docker` group effectively grants high privileges and should not be done merely to silence an error.
5. **Validate logging end-to-end.** Agent service running ≠ agent enrolled; agent enrolled ≠ Docker/pfSense logs received; a saved custom rule ≠ alert triggered.
6. **Protect infrastructure access.** Keep Proxmox, pfSense, Portainer, Wazuh and Nessus interfaces off public-facing networks; rotate default credentials; restrict access to authorized lab users.

## Next milestones

- [ ] Confirm pfSense is agent 003 (or the correct assigned ID) and **Active** in Wazuh.
- [ ] Generate one controlled pfSense `block` rule event and confirm it appears with the appropriate Wazuh rule.
- [ ] Generate a Docker-specific event and verify its structured attributes in Threat Hunting.
- [ ] Confirm Nessus scan creation works and run a **scoped scan** only against a controlled lab target after validating isolation.
- [ ] Export Nessus results without exposing credentials or external endpoints.
- [ ] Capture proof of post-reboot Wazuh and Nessus service health.
- [ ] Review screenshots one final time for hostnames, local network details, user accounts, license codes and sensitive values before publishing.
