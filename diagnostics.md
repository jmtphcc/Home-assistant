# Diagnostics

For the current state and constraints, start with [the handoff](README.md).

| Component | Evidence and interpretation |
| --- | --- |
| Windows host | Native Windows PowerShell and VirtualBox on a laptop. The command runner has a standard Windows token. Some service, process, and VirtualBox COM queries returned access denied. Interactive VirtualBox Manager worked. |
| Wi-Fi and routing | eero provides DHCP and is the Windows host's default gateway. The upstream AT&T BGW320-500 was identified from its local status page. No consumer eero flow export was verified. The Windows laptop is an ordinary Wi-Fi client. |
| Home Assistant VM | Home Assistant OS runs under VirtualBox with NAT and a bridged Wi-Fi interface. The guest listens on HTTP port 80. Windows localhost port 8123 correctly forwards to guest port 80. A trial forwarding to guest port 8123 failed and was reversed. |
| Local verification | The Windows loopback and bridged guest frontend returned HTTP 200. The unauthenticated API returned HTTP 401, confirming the app responded and required authentication. |
| Remote verification | Tailscale Serve forwards TCP 8123 to the Windows loopback listener. The user confirmed the phone connected. Following the last Core restart, the host verified HTTP 200 through both local and Tailscale addresses. The other laptop and full-host reboot remain untested. |
| Windows Remote Desktop | Windows Pro is configured to accept RDP. The observed listener responds on TCP 3389. Home Assistant's HTTP port 8123 cannot be used as an RDP endpoint. |
| AdGuard Home | The add-on originally listened on guest loopback only. Restarting it after the LAN was ready made an explicit LAN DNS query succeed and appear in Query Log. No default DNS was changed. LAN binding after a full VM reboot remains unverified. |
| NetAlertX | The one-pass pilot found LAN hosts. The saved count included VM-internal entries. The add-on startup wrapper can re-add interfaces and run discovery probes. Its sidebar shortcut remains and the app was stopped. No MQTT publication was configured. |
| Built-in Nmap Tracker | Setup was cancelled before activation. The active-device suppression setting is distinct from overall discovery cadence, and the documented source caps the configurable interval at one hour. |
| eero integration | Custom Marketplace release 1.8.1 was installed and Core restarted successfully. The owner uses Amazon sign-in, which the integration does not support. No eero credentials, admin accounts or router settings were changed. |
| Existing devices | The main registry already includes an LG webOS TV, Ring cameras and a doorbell, and a guest-room TV through Google Cast. Android TV Remote separately discovered that guest-room TV. WLED entries were retrying setup. The two holiday plugs were not found under their eero labels. |

Use existing device integrations for control. For uncertain matches, get the device model or match MAC addresses locally with the owner. Recorded eero counts included offline devices and are not directly comparable to the scanner's inventory.
