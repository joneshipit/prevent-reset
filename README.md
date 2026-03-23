# Prevent Reset

Silently blocks "Erase All Content and Settings" on macOS. The option looks completely normal — it just fails when clicked. No greyed-out buttons, no "managed by admin" messages.

Perfect for family Macs where someone might accidentally factory reset (and brick the machine by unplugging mid-reset).

## How It Works

Installs two lightweight LaunchDaemons that kill the Erase Assistant process whenever it launches:

1. **Event-driven** — uses launchd's `WatchPaths` to trigger only when Erase Assistant is accessed. Zero CPU usage during normal use.
2. **Fallback** — runs `pkill` every 5 seconds as a safety net. Negligible overhead.

The user keeps full admin access — they can install apps, change settings, do everything. They just can't wipe the Mac.

## Install

```bash
curl -L https://raw.githubusercontent.com/joneshipit/prevent-reset/main/lock-down-mac.sh -o lock-down-mac.sh && chmod +x ./lock-down-mac.sh && sudo ./lock-down-mac.sh
```

## Uninstall

When a reset is actually needed, run this to remove the protection:

```bash
curl -L https://raw.githubusercontent.com/joneshipit/prevent-reset/main/unlock-mac.sh -o unlock-mac.sh && chmod +x unlock-mac.sh && sudo ./unlock-mac.sh
```

## What It Does

| | With protection | Without |
|---|---|---|
| "Erase All Content and Settings" | Visible but silently fails | Works normally |
| Admin access | Full | Full |
| App installs | Works | Works |
| System settings | Works | Works |
| Recovery Mode (Apple Silicon) | Requires user auth (built-in) | Same |
| Recovery Mode (Intel) | Optional firmware password | No protection |

## Credits

Made by [joneshipit](https://github.com/joneshipit)
