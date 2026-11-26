# Home Network Troubleshooting: "No Internet on One or More Devices"

A step-by-step method I use for diagnosing home and small-shop network problems. The addresses below are generic examples.

## Scenario
A small network with one ISP modem/router, a few Windows PCs and phones on Wi-Fi, and one device that needs a port opened. Symptom: some devices show "Connected, no internet".

## Method
1. **Scope the problem.** One device or all? Wired or Wi-Fi? Started after a change?
2. **Check the physical layer.** Cables, link lights, power cycle of the modem and router, in that order.
3. **Check the device's IP settings.** Run `ipconfig /all` on Windows. A 169.254.x.x address means no DHCP reply; a wrong gateway means a configuration problem.
4. **Test step by step.**
   - `ping 192.168.1.1` (router/LAN)
   - `ping 8.8.8.8` (internet, bypassing DNS)
   - `ping google.com` (DNS)

   The first failure tells you which layer is broken: LAN, WAN or DNS.
5. **Check the router.** Confirm the WAN connection is up, DHCP is on and the address pool has free addresses, and no IP conflicts exist.
6. **Fix and verify.** Renew the lease (`ipconfig /release` then `/renew`), flush DNS (`ipconfig /flushdns`), or set a known DNS server. Re-run the three pings.

## Opening a port safely (port forwarding)
1. Give the target device a fixed (reserved) IP so the rule does not break.
2. Create the forwarding rule: external port, internal IP, internal port, protocol (TCP/UDP).
3. Open only the port that is needed, and only for the service that needs it.
4. Test from outside the network, then remove the rule if it is no longer needed.

## Basic security checks
- Change the router's default admin password.
- Use WPA2/WPA3 with a strong Wi-Fi password.
- Turn off remote administration unless it is needed.
- Keep router firmware updated.
- Review connected devices and remove unknown ones.

## VPN basics
- Use a VPN to connect securely to a remote network or to protect traffic on public Wi-Fi.
- Install and configure the VPN client on Windows, then confirm the connection and the new IP address.
- If it fails, check internet access first, then credentials, then the VPN server address and the firewall.

## Firewall basics
- Keep Windows Defender Firewall on for all network profiles.
- Allow an app or open a specific port only when a service needs it, and remove the rule when it is no longer needed.
- If an app or connection fails, check whether the firewall is blocking it before changing other settings.

## Tools used
Windows Command Prompt (`ipconfig`, `ping`, `tracert`, `nslookup`), router admin page.
