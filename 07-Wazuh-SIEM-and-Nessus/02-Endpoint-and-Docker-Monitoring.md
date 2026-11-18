# 02 — Kali, Ubuntu and Docker Monitoring

## Goal

Use Wazuh to monitor endpoint activity on **Kali (VM 202)** and **Ubuntu Docker Server (VM 203)**, with both agents reporting to manager `10.10.1.52` on TCP port `1514`.

## Kali Agent 001

Kali's configuration showed:

```xml
<client>
  <server>
    <address>10.10.1.52</address>
    <port>1514</port>
    <protocol>tcp</protocol>
  </server>
</client>
```

The Wazuh dashboard previously showed **Kali active** (Agent `001`).

[Evidence: Kali agent configuration](screenshots/wazuh/02-kali-agent-manager-config.png)

## Ubuntu Agent 002

Initially, Ubuntu's `ossec.conf` pointed at the wrong manager address (`10.0.0.2`). The configuration was updated to `10.10.1.52`, and the service was restarted. The dashboard later identified **Ubuntu as active**, agent `002`, at `10.10.30.50`.

Relevant inspection and recovery commands:

```bash
sudo grep -A 10 '<client>' /var/ossec/etc/ossec.conf
sudo systemctl restart wazuh-agent
sudo systemctl is-active wazuh-agent
```

[Evidence: original address](screenshots/wazuh/03-ubuntu-original-wrong-address.png) · [Evidence: agent active](screenshots/wazuh/05-ubuntu-agent-active.png)

## Docker Listener

Ubuntu's Wazuh configuration included:

```xml
<wodle name="docker-listener">
  <disabled>no</disabled>
</wodle>
```

The Wazuh log then reported:

```text
INFO: Module docker-listener started.
INFO: Starting to listening Docker events.
```

The unprivileged `docker run` command initially failed with socket permission denied; `sudo docker run --rm hello-world` succeeded. This established that Docker itself was functioning, **not** that Wazuh ingested a Docker security event.

A dashboard search for `docker` returned **three sudo-related alerts**, not clearly identified Docker container event alerts. Docker ingestion remains **unverified** pending an event test.

Useful diagnostics:

```bash
sudo grep -i 'docker-listener' /var/ossec/logs/ossec.log | tail -n 20
sudo docker run --rm hello-world
```

## Evidence

- [Docker listener XML](screenshots/wazuh/04-docker-listener-config.png)
- [Docker listener startup messages](screenshots/wazuh/06-docker-listener-started.png)
- [Docker hello-world test](screenshots/wazuh/07-docker-hello-world-test.png)
- [Ubuntu Threat Hunting alerts](screenshots/wazuh/08-ubuntu-threat-hunting.png)
- [Docker search not sufficient to prove integration](screenshots/wazuh/09-docker-search-sudo-only.png)

## Follow-up

- [ ] Generate a distinct authorized Docker start/stop event.
- [ ] Find a matching Wazuh event with Docker-specific fields / rule group.
- [ ] If not found, inspect agent Docker API permissions, manager ingestion, and event rules.
- [ ] Avoid treating all matches to the word `docker` as evidence of Docker event collection.
