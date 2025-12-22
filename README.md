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

**Important: Laptop is configured to shutdown on lid close**

This system uses hybrid graphics (AMD + NVIDIA RTX 3060) with on-demand GPU switching. When resuming from suspend, the display connector names can change, causing GNOME to reset display configuration (resolution, scaling, color settings).

**Current Configuration:**
- Lid close (AC power): Shutdown
- Lid close (Battery): Shutdown

This prevents display configuration resets that occur with suspend/resume on hybrid graphics systems.

**To change this behavior:**
```bash
# View current settings
gsettings get org.gnome.settings-daemon.plugins.power lid-close-ac-action
gsettings get org.gnome.settings-daemon.plugins.power lid-close-battery-action

# Options: 'suspend', 'shutdown', 'hibernate', 'nothing', 'blank'
gsettings set org.gnome.settings-daemon.plugins.power lid-close-ac-action 'suspend'
gsettings set org.gnome.settings-daemon.plugins.power lid-close-battery-action 'suspend'
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


