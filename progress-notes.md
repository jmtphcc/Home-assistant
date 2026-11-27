# Progress notes

For setup details, see [the handoff](README.md) and [diagnostics](diagnostics.md).

## Completed

- Verified local Windows execution and documented VirtualBox, guest networking, and Tailscale.
- Restored the correct guest port and verified local Home Assistant HTTP and normal API authentication.
- Enabled private Tailscale Serve with user approval. Verified phone connectivity with the user.
- Verified that the existing AdGuard Home add-on can resolve and log a direct LAN DNS lookup after an add-on restart.
- Installed NetAlertX and confirmed discovery. Stopped it when recurring activity could not be verified against the approved limits.
- Installed the eero community integration. Checked that an automatic backup existed, restarted Core only, and verified both access paths recovered.
- Matched recognizable entries from eero screenshots against the existing Home Assistant registry. No devices were added or renamed during that comparison.

## Pending

- Choose a daily or on-demand inventory method that can be verified to stay within the main eero LAN. A candidate custom scanner was discussed but not installed.
- If using the eero integration, decide whether to authorize its documented separate-admin workaround or leave the integration unconfigured.
- Identify holiday plug models and resolve ambiguous device names before adding controls. Verify existing integrations to avoid duplicates.
- When the owner's other laptop is on home Wi-Fi, run the one-device AdGuard pilot and verify browsing domains in Query Log. Revert that device to automatic DNS to end the pilot.
- Verify access from the other laptop and home desktop, and verify availability after a planned server restart if requested.
