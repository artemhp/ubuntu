# Ubuntu Configuration

This repository tracks the configuration and adjustments made to my Ubuntu setup.


## Terminal Customization

*   **Theme:** Built-in "Gray on Black" scheme.
*   **Color Palette:** Built-in "Solarized" palette.
*   **Font:** DejaVu Sans Mono.

## Must-Have Tools

*   Gemini CLI
*   Antigravity
*   VSCode
*   Node.js
*   Brave (Nightly)
*   Google Chrome (Dev)
*   Transmission
*   Telegram
*   Gimp
*   Gnome Tweaks

## Power Management

This system uses hybrid graphics (AMD + NVIDIA RTX 3060) with on-demand GPU switching. When resuming from suspend, the display connector names can change, causing GNOME to reset display configuration (resolution, scaling, color settings).

**Current Configuration:**
- Lid close (AC power): Nothing (keeps running)
- Lid close (Battery): Nothing (keeps running)
- logind: `HandleLidSwitch=ignore`, `HandleLidSwitchExternalPower=ignore`

This prevents display configuration resets that occur with suspend/resume on hybrid graphics systems, and allows using an external monitor with the lid closed.

**To change lid close behavior (GNOME):**
```bash
# View current settings
gsettings get org.gnome.settings-daemon.plugins.power lid-close-ac-action
gsettings get org.gnome.settings-daemon.plugins.power lid-close-battery-action

# Options: 'suspend', 'shutdown', 'hibernate', 'nothing', 'blank'
gsettings set org.gnome.settings-daemon.plugins.power lid-close-ac-action 'nothing'
gsettings set org.gnome.settings-daemon.plugins.power lid-close-battery-action 'nothing'
```

Note: Both GNOME gsettings AND `/etc/systemd/logind.conf` must be set — logind controls system-level events, GNOME controls session-level actions (and can override logind via the API).

### Using Laptop with Closed Lid + External Monitor

To prevent sleep when the lid is closed (e.g. using an external monitor), edit `/etc/systemd/logind.conf`:

```bash
# Uncomment and set these lines in /etc/systemd/logind.conf:
# HandleLidSwitch=ignore
# HandleLidSwitchExternalPower=ignore
```

Then apply without rebooting:

```bash
sudo systemctl restart systemd-logind
```

### Keyboard Wakeup from Suspend

By default, the ASUS ITE keyboard controller (`ITE Device(8910)`) has wakeup enabled, but its parent xHCI USB controller has wakeup disabled — blocking keyboard wakeup.

Fix with a udev rule:

```bash
sudo tee /etc/udev/rules.d/90-keyboard-wakeup.rules > /dev/null << 'EOF'
# Enable wakeup for the USB controller hosting the ASUS ITE keyboard
ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="048d", ATTR{power/wakeup}="enabled"
# Enable wakeup for the parent xHCI host controller (usb1)
ACTION=="add", SUBSYSTEM=="usb", KERNEL=="usb1", ATTR{power/wakeup}="enabled"
EOF

sudo udevadm control --reload-rules
```

## Google Drive

Google Drive is mounted using rclone to provide proper file/folder names in VSCode and terminal.

**Mount Location:** `~/GoogleDrive`

See [google-drive-setup.md](google-drive-setup.md) for complete configuration details.

## Ollama

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull the gpt-oss model
ollama pull gpt-oss

# Run the gpt-oss model
ollama run gpt-oss
```


