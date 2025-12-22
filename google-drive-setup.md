# Google Drive Configuration with rclone

## Overview
Google Drive is mounted using rclone to provide proper file/folder names (instead of Google's internal IDs used by GVFS).

## Mount Location
- **Primary mount**: `~/GoogleDrive`
- **Symlink**: `~/Projects/GoogleDrive` → `~/GoogleDrive`

## Installation Steps

### 1. Fix Broken PPA (if needed)
```bash
sudo add-apt-repository --remove ppa:mitchellaugustin/asusctl
```

### 2. Install rclone
```bash
sudo apt update && sudo apt install -y rclone
```

### 3. Configure rclone with Google Drive
```bash
rclone config
```

Follow these steps:
- Press `n` for "New remote"
- Enter `gdrive` as the name
- Choose `drive` (Google Drive)
- Leave `client_id` blank (press Enter)
- Leave `client_secret` blank (press Enter)
- For `scope`, choose `1` (full access)
- Leave `root_folder_id` blank (press Enter)
- Leave `service_account_file` blank (press Enter)
- For `Edit advanced config`, choose `n` (no)
- For `Use auto config`, choose `y` (yes) - browser will open
- Sign in to Google account and grant permissions
- Press `y` to confirm
- Press `q` to quit

### 4. Create Mount Point
```bash
mkdir -p ~/GoogleDrive
```

### 5. Manual Mount (one-time)
```bash
rclone mount gdrive: ~/GoogleDrive --vfs-cache-mode writes --daemon
```

## Auto-Mount on Login

### Systemd Service
The service is configured at: `~/.config/systemd/user/rclone-gdrive.service`

### Service Configuration
```ini
[Unit]
Description=RClone Google Drive Mount
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
ExecStartPre=/bin/mkdir -p %h/GoogleDrive
ExecStart=/usr/bin/rclone mount gdrive: %h/GoogleDrive \
    --vfs-cache-mode writes \
    --vfs-cache-max-age 72h \
    --vfs-read-chunk-size 128M \
    --vfs-read-chunk-size-limit off \
    --buffer-size 256M
ExecStop=/bin/fusermount -u %h/GoogleDrive
Restart=on-failure
RestartSec=10

[Install]
WantedBy=default.target
```

### Enable Auto-Mount
```bash
systemctl --user daemon-reload
systemctl --user enable rclone-gdrive.service
```

## Auto-Mount Behavior
- **Starts on**: User login (not OS boot)
- **Enabled**: Yes, will auto-start on every login
- **User-level**: Mounts only when you're logged in
- **Unmounts**: When you log out

## Useful Commands

### Check mount status
```bash
systemctl --user status rclone-gdrive.service
```

### Check if mounted
```bash
mount | grep GoogleDrive
```

### List mounted files
```bash
ls ~/GoogleDrive
```

### Manually start service
```bash
systemctl --user start rclone-gdrive.service
```

### Manually stop service
```bash
systemctl --user stop rclone-gdrive.service
```

### Manually unmount
```bash
fusermount -u ~/GoogleDrive
```

### Restart service
```bash
systemctl --user restart rclone-gdrive.service
```

### Disable auto-mount
```bash
systemctl --user disable rclone-gdrive.service
```

### View service logs
```bash
journalctl --user -u rclone-gdrive.service -f
```

## Usage

### VSCode
```bash
code ~/GoogleDrive
# or
code ~/Projects/GoogleDrive
```

### Terminal
```bash
cd ~/GoogleDrive
```

## Advantages over GVFS
- **Proper file names**: Shows actual file/folder names instead of Google's internal IDs
- **VSCode compatible**: Works seamlessly with VSCode and other editors
- **Command-line friendly**: Full access via terminal
- **Better performance**: Caching and optimization options

## Important Notes
- Files are downloaded on-demand when accessed
- Cache settings optimize performance for frequently accessed files
- Service automatically restarts on failure
- Mount point is created automatically if it doesn't exist
