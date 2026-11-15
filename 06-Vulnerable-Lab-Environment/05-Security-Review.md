# 05 — Security Review and Verification Checklist

## Current status

The lab environment successfully runs several intentionally vulnerable systems. **It is not yet verified as isolated or safe for unrestricted testing.** This page separates what was observed from what remains to be tested.

| Area | Confirmed | Remaining work |
|---|---|---|
| Metasploitable2 VM | Guest booted, received `10.10.10.50/24`, gateway ping succeeded | Restrict outbound internet and unwanted VLAN routes |
| pfSense VLANs | Previous lab documentation shows VLAN 10/20/30 created and assigned | Validate interface IPs, route policy, logging, and actual deny rules |
| VLAN 30 containers | Three vulnerable apps connected to `vlan30` | Confirm enforcement of VLAN separation and authorized reachability only |
| WebGoat | Login loads and Docker shows `healthy` | Make the missing `curl` dependency fix permanent |
| Application accounts | Login pages displayed for bWAPP and WebGoat | Set up unique training credentials if needed; do not publish passwords |
| Web protocols | HTTP web apps reachable inside the lab | Ensure no public exposure; use only in trusted isolated environment |

## Key security observations

**1. Metasploitable2 had outbound internet connectivity.** A successful `google.com` ping demonstrated that this intentionally vulnerable machine could reach a public endpoint at the time of testing. This is an **open security concern**, not evidence of isolation.

**2. VLAN membership alone is not a firewall policy.** Portainer shows Docker VLAN 30 configuration; earlier pfSense rules permitted IPv4 traffic to any destination. Proper deny/allow rules and test evidence are necessary before claiming the networks are isolated.

**3. Vulnerable applications are intended only for training.** Metasploitable2, DVWA, bWAPP and WebGoat are designed to contain weaknesses. They should never be publicly forwarded or treated as production services.

**4. Container health is not a security guarantee.** WebGoat became `healthy` after adding `curl`, but Docker health only shows that its check succeeded. It does not prove the application is patched or securely configured.

**5. HTTP access should remain within the trusted test environment.** The WebGoat training endpoint served HTTP on port 8080. No encryption should be assumed for credentials or session traffic.

## Next validation tasks

- [ ] Confirm VM 204 has no direct access to the physical/home network or public internet.
- [ ] Review pfSense VLAN 10 and VLAN 30 firewall policies, then add restrictive rules appropriate for the training lab.
- [ ] Test allowed and denied communication from Kali to the vulnerable systems, and between the vulnerable VLANs.
- [ ] Verify the effective subnet masks and routing on VLAN 30, rather than treating calculator exercises as live configuration.
- [ ] Persist the WebGoat health-check dependency fix in a custom image or redesign the check, and test a fresh container deployment.
- [ ] Verify the separate DVWA Docker instance's web interface (the currently captured DVWA login page is on Metasploitable2).
- [ ] Confirm admin passwords have been changed from defaults on infrastructure management platforms.
- [ ] Confirm there are no public port forwards, exposed credentials, private keys, or authentication tokens before publishing repository material.

## Portfolio statement

This stage demonstrates virtualized system deployment, IPv4 troubleshooting, Docker networking, container health-check investigation, and recognition of lab security boundaries. Further security validation is planned; **the current evidence should not be described as proof of hardened isolation**.
