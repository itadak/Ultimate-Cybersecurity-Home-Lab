# 03 — pfSense Wazuh Agent, Firewall Logs and Custom Rule

## Goal

Forward firewall monitoring information from pfSense VM `201` (`pfSense.home.arpa`, LAN `10.10.1.254`) to Wazuh Manager (`10.10.1.52`).

## Installation differences from the video

The linked video used Wazuh Agent `4.7.2`. On the lab's pfSense 2.9.0 instance, `pkg search wazuh-agent` displayed `wazuh-agent-4.14.10`, but requesting `4.7.2` failed because that version was not available from the configured repositories. A later package attempt emitted a **FreeBSD OS version mismatch warning**. I did **not** treat bypassing the version mismatch as a safe fix.

The Wazuh dashboard previously listed pfSense as a **disconnected 4.7.2 agent**. That and a newer version appearing in package search are not proof of a successful 4.14.10 installation. The final installed pfSense agent version and final Wazuh **Active** state need verification.

## Agent and manager configuration

The pfSense agent's XML pointed at:

```xml
<server>
  <address>10.10.1.52</address>
  <port>1514</port>
  <protocol>tcp</protocol>
</server>
```

Its background services reported running when checked with:

```sh
service wazuh-agent status
```

## Permission issue discovered and repaired

A repeated error appeared in `/var/ossec/logs/ossec.log`:

```text
ERROR: (1103): Could not open file 'etc/shared/merged.mgt' due to [(13)-(Permission denied)].
```

Inspection showed `/var/ossec/etc/shared` owned by `root:wazuh`, with group read/execute but **no group write**, and the `merged.mgt` file did not yet exist. The directory was changed from `drwxr-xr-x` to `drwxrwx---`:

```sh
ls -ld /var/ossec/etc/shared
chmod 770 /var/ossec/etc/shared
ls -ld /var/ossec/etc/shared
service wazuh-agent restart
```

The restart subsequently reported success, and the immediate new log tail showed running modules rather than repeated permission errors. **No screenshot definitively proves the agent appeared Active afterward.**

## Centralized pfSense log collection

A shared Wazuh agent configuration contained the following block, intended to collect pfSense firewall log entries:

```xml
<agent_config>
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/filter.log</location>
  </localfile>
</agent_config>
```

A custom rule was drafted in Wazuh using group `pfsense`, `if_sid` `87700`, rule ID `87701`, and description `pfSense firewall drop event`. **Rule creation in the editor is not the same as validation:** save confirmation, matching event data and a successful ruleset test still require evidence. The event format and `if_sid` must be checked against the real logs.

## Evidence

- [Available package](screenshots/pfsense/01-package-search.png)
- [FreeBSD package compatibility warning](screenshots/pfsense/02-freebsd-version-mismatch.png)
- [Agent manager address](screenshots/pfsense/03-manager-address-config.png)
- [Previously disconnected in dashboard](screenshots/pfsense/09-firewall-agent-disconnected.png)
- [Agent components running](screenshots/pfsense/10-agent-service-running.png)
- [Permission denied logs](screenshots/pfsense/11-merged-mgt-permission-denied.png)
- [Directory permissions before fix](screenshots/pfsense/12-shared-directory-before-fix.png)
- [Permission change](screenshots/pfsense/13-shared-directory-permission-change.png)
- [Agent restart](screenshots/pfsense/14-agent-restart-log.png)
- [Shared config](screenshots/pfsense/15-shared-firewall-log-config.png)
- [Custom rule editor](screenshots/pfsense/16-custom-pfsense-rule.png)

## Follow-up

- [ ] Confirm the exact installed version with `pkg info wazuh-agent`.
- [ ] Confirm agent is **Active** in Wazuh after reconnecting and restarting.
- [ ] Prove firewall logs from `/var/log/filter.log` are ingested by Wazuh.
- [ ] Validate custom rule ID 87701 against a **real lab-only firewall drop event** using Wazuh's ruleset test and event search.
- [ ] Reassess the pfSense package installation method; avoid OS-version bypasses and unmanaged FreeBSD package upgrades.
- [ ] Export the current pfSense configuration before future package or firewall changes.
