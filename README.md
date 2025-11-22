# Ubuntu Configuration

This repository tracks the configuration and adjustments made to my Ubuntu setup.

## Terminal Customization

*   **Theme:** Built-in "Gray on Black" scheme.
*   **Color Palette:** Built-in "Solarized" palette.
*   **Font:** FreeMono.

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

## Installation Commands

### 1. System Update
First, ensure your package list is up to date.
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Brave Browser (Nightly)
```bash
sudo apt install curl apt-transport-https -y
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-nightly.s3.brave.com/brave-browser-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=amd64] https://brave-browser-apt-nightly.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-nightly.list
sudo apt update
sudo apt install brave-browser-nightly -y
```

### 3. Google Chrome (Dev Channel)
**Note:** Google Chrome Canary is not available for Linux. The Dev channel is the most unstable version available.
```bash
sudo apt install curl -y
curl -fSsL https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/google-chrome.gpg > /dev/null
echo "deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
sudo apt update
sudo apt install google-chrome-unstable -y
```

### 4. VSCode
```bash
sudo apt install software-properties-common apt-transport-https wget -y
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt update
sudo apt install code -y
```

### 5. Node.js (via NodeSource)
This will install Node.js 20.x. You can change the `setup_20.x` to another version if needed.
```bash
sudo apt install curl -y
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

### 6. Gimp & Transmission
These are available in the default Ubuntu repositories.
```bash
sudo apt install gimp transmission-gtk -y
```

### 7. Telegram
Installed via Snap for easy updates.
```bash
sudo snap install telegram-desktop
```

### 8. Gemini CLI & Antigravity
*   **Gemini CLI:** Installation instructions to be added.
*   **Antigravity:** Installation instructions to be added.

