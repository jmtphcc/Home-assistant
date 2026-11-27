# Home Assistant handoff

## Goals and constraints

Make the home hub reliably accessible over Tailscale, identify home-network devices, bring useful controls into Home Assistant, and pilot DNS domain logging. Keep Windows and remote access available while the owner is away. Ask before changing system, network or integration settings. Prefer a single daily LAN discovery pass or an on-demand scan. Preserve the current eero DNS and DHCP settings during tests.

## Working setup

- Windows 11 Pro laptop runs VirtualBox 7.2.20 and the Home Assistant OS VM named home assistant fresh. Its first adapter uses NAT and the second is bridged over Wi-Fi to the main eero LAN. The AT&T BGW320-500 is upstream of eero.
- Home Assistant serves HTTP on guest port 80. VirtualBox forwards host loopback TCP 8123 to guest TCP 80. The original destination was restored after testing confirmed that guest port 8123 did not answer.
- Tailscale Serve forwards tailnet TCP 8123 to Windows loopback TCP 8123. Android reached Home Assistant over Tailscale. Home Assistant returned HTTP 200 on the local and Tailscale endpoints after the last Core restart. Windows remained available. Get the current addresses from the owner or current host configuration.
- Home Assistant Core can restart independently of Windows, VirtualBox and Tailscale. A recent automatic Home Assistant backup was visible before the last Core restart.
- Windows Remote Desktop listens on the default TCP port 3389. Use the server's Tailscale address in Windows App. Use port 8123 in Home Assistant or a web browser.

## AdGuard Home

The Home Assistant AdGuard app was already installed (6.3.2). Its initial DNS listener was loopback-only. Restarting only that app after the VM network was ready made DNS answer at the guest LAN address. A direct lookup of example.com from Windows appeared in AdGuard's Query Log. Protection was on during the successful test. No client default DNS or router settings were changed. The planned next step is a reversible one-device pilot on the owner's other laptop when it is on home Wi-Fi. DNS logs show requests sent to AdGuard, including background activity. Continuous logging and post-VM-reboot DNS binding are unverified.

## Device discovery and eero

- NetAlertX 26.10.28 was installed from alexbelgium's Home Assistant add-on repo. It found LAN devices and showed 54 new entries including internal VM records. The dashboard shortcut was added. The app is stopped and start-on-boot was left off. No MQTT data was published.
- The add-on wrapper can probe and re-add VM interfaces at startup. The one-daily-pass schedule was not verified. Home Assistant's built-in Nmap Tracker was considered and then cancelled before setup because its configurable scan interval is capped at one hour. A separate custom scanner was discussed but was not installed.
- The schmittx eero integration v1.8.1 was installed from Marketplace, followed by a successful Core restart. The owner uses Amazon sign-in. The integration supports direct eero login and is unconfigured. Creating or inviting a separate eero admin is a documented workaround and has not been authorized or done.
- Eero app screenshots showed 64 unassigned entries including offline devices. Overlapping screenshots cover only part of the list and provide names but no IP/MAC details. Match duplicates by MAC wherever practical. Keep uncertain matches unlabeled.
- Existing Home Assistant registry matches include the living-room LG webOS TV and Ring's two floodlight cameras and doorbell. The guest-room TV already appears via Google Cast and was separately discovered by Android TV Remote. WLED-Gledopto is a likely match and the WLED integration reported setup retries. The Christmas plugs had no matching registry entries by name and may exist under other names. Their models or MAC addresses are the best next evidence.
- Use the registry and device-specific integrations for controls. Prefer verifying existing devices before adding duplicates. Eero labels or an inventory scan alone do not create controls.

## References

- [Home Assistant on Windows](https://www.home-assistant.io/installation/windows/)
- [Tailscale Serve](https://tailscale.com/docs/reference/tailscale-cli/serve)
- [AdGuard Home community app](https://github.com/hassio-addons/repository/blob/master/adguard/DOCS.md)
- [NetAlertX Home Assistant integration](https://github.com/netalertx/NetAlertX/blob/main/docs/HOME_ASSISTANT.md)
- [NetAlertX add-on startup script](https://github.com/alexbelgium/hassio-addons/blob/master/netalertx/rootfs/etc/cont-init.d/91-configure.sh)
- [Nmap Tracker configuration](https://github.com/home-assistant/core/blob/dev/homeassistant/components/nmap_tracker/config_flow.py)
- [Eero custom integration](https://github.com/schmittx/home-assistant-eero)
- [Network Scanner custom integration](https://github.com/parvez/network_scanner)
