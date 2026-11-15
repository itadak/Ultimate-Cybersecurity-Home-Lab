# Screenshot Index

These are images captured during **Video 2** of the personal lab. Local addresses are included as configuration evidence. Host-specific MAC identifiers and visible example default-password hints have been obscured in selected images.

## Metasploitable2

- [01-download-extract-convert.png](screenshots/metasploitable/01-download-extract-convert.png) — Metasploitable2 ZIP extraction, virtual disk preparation and QCOW2 conversion
- [02-storage-error.png](screenshots/metasploitable/02-storage-error.png) — Proxmox start failure caused by storage image content-type restriction
- [03-proxmox-vm-hardware.png](screenshots/metasploitable/03-proxmox-vm-hardware.png) — VM 204 hardware, disk attachment, VLAN-tagged adapter
- [04-console-ip-and-connectivity.png](screenshots/metasploitable/04-console-ip-and-connectivity.png) — Guest `10.10.10.50/24` and successful ping statistics
- [05-initial-missing-ipv4.png](screenshots/metasploitable/05-initial-missing-ipv4.png) — Before IPv4 configuration was available on eth0
- [06-vlan10-gateway-ping.png](screenshots/metasploitable/06-vlan10-gateway-ping.png) — Guest ping to VLAN 10 gateway
- [07-browser-address-mismatch.png](screenshots/metasploitable/07-browser-address-mismatch.png) — Browser connection attempted to the wrong example guest IP

## Networking and subnetting

- [01-subnet-10-10-30-0-24.png](screenshots/networking/01-subnet-10-10-30-0-24.png) — /24 subnet calculation
- [02-subnet-10-10-30-150-27.png](screenshots/networking/02-subnet-10-10-30-150-27.png) — /27 subnet calculation exercise
- [03-bwapp-vlan30.png](screenshots/networking/03-bwapp-vlan30.png) — Portainer bWAPP VLAN 30 IP and gateway
- [04-dvwa-vlan30.png](screenshots/networking/04-dvwa-vlan30.png) — Portainer DVWA VLAN 30 IP and gateway
- [05-webgoat-vlan30.png](screenshots/networking/05-webgoat-vlan30.png) — Portainer WebGoat VLAN 30 IP and gateway

## Docker applications

- [01-portainer-initial-webgoat-unhealthy.png](screenshots/docker-apps/01-portainer-initial-webgoat-unhealthy.png) — Initial container inventory with WebGoat unhealthy
- [02-portainer-containers-healthy.png](screenshots/docker-apps/02-portainer-containers-healthy.png) — Final container inventory with WebGoat healthy
- [03-dvwa-login-metasploitable2.png](screenshots/docker-apps/03-dvwa-login-metasploitable2.png) — DVWA login page on Metasploitable2, not standalone DVWA container
- [04-nginx-test-page.png](screenshots/docker-apps/04-nginx-test-page.png) — Nginx test web page
- [05-webgoat-login.png](screenshots/docker-apps/05-webgoat-login.png) — WebGoat login loaded over HTTP
- [06-bwapp-login.png](screenshots/docker-apps/06-bwapp-login.png) — bWAPP login loaded over HTTP

## Docker and application troubleshooting

- [01-webgoat-startup-logs.png](screenshots/troubleshooting/01-webgoat-startup-logs.png) — Tomcat and WebGoat startup messages
- [02-healthcheck-curl-missing.png](screenshots/troubleshooting/02-healthcheck-curl-missing.png) — Docker health result `/bin/sh: curl: not found`
- [03-healthcheck-definition.png](screenshots/troubleshooting/03-healthcheck-definition.png) — Docker health-check command definition
- [04-http-client-check.png](screenshots/troubleshooting/04-http-client-check.png) — No `wget` or `busybox` found in the container
- [05-container-operating-system.png](screenshots/troubleshooting/05-container-operating-system.png) — Container guest operating system Ubuntu 24.04.5
- [06-curl-installation-complete.png](screenshots/troubleshooting/06-curl-installation-complete.png) — Package manager completed curl installation
- [07-healthcheck-healthy.png](screenshots/troubleshooting/07-healthcheck-healthy.png) — Docker health status changed to healthy
- [08-https-on-http-port-error.png](screenshots/troubleshooting/08-https-on-http-port-error.png) — HTTPS on WebGoat HTTP port failed
