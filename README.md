# ansible-arch-installer

An Ansible playbook to install and configure Arch Linux (Intel CPU + Intel GPU, Qtile/Xorg).

## Setup

1. Clone this repo
2. Update `inventory/hosts.yaml` with the IP address of the target machine
3. Edit values in `inventory/group_vars/arch.yml`
4. Create host vars: `ansible-vault create inventory/host_vars/remote_system.yml` with

```yaml
luks_pass: ""
user_pass: ""
```

## Usage

After booting from the Arch installation media, set the root password using `passwd`.

Then connect to WiFi:

1. `iwctl`
2. `device list`
3. `station wlan0 scan`
4. `station wlan0 get-networks`
5. `station wlan0 connect <SSID>`

Optional – wipe disk:

```bash
dd if=/dev/urandom of=/dev/sdX bs=4096 iflag=fullblock status=progress
```

Run the bootstrap from your local machine:

```bash
ansible-playbook playbook.yml --ask-vault-password -t bootstrap
```

After booting into the installed system, reconnect to WiFi:

```bash
nmcli dev status
nmcli dev wifi list
sudo nmcli dev wifi connect <SSID> --ask
```

Then run the main setup:

```bash
ansible-playbook playbook.yml --ask-vault-password -t mainsetup
```

---

## Package Overview

Full target package list for a new laptop. Packages are categorized by installation method.

### Optional AUR packages

The playbook installs packages with `pacman` only, so AUR packages are **not**
installed automatically. Install this apps manually with an AUR helper
(`yay`, `paru`, …) if you want them:

| Package                | Description                  | Role      |
| ---------------------- | ---------------------------- | --------- |
| betterbird-bin         | Thunderbird fork (email)     | apps/base |
| enpass-bin             | Password manager             | apps/base |
| google-chrome          | Web browser (Chromium-based) | apps/base |
| joplin-desktop         | Markdown notes app           | apps/base |
| nextcloud-client       | Nextcloud sync client        | apps/base |
| signal-desktop         | Signal messenger             | apps/base |
| simplescreenrecorder   | Screen recorder              | apps/base |
| visual-studio-code-bin | VS Code editor               | apps/base |

> To list the AUR (foreign) packages already installed on a system, run
> `pacman -Qm` (or `pacman -Qmq` for names only).

### Already integrated in Ansible

These packages are already managed by existing roles in this project:

#### Bootstrap (pacstrap via `base_packages` role)

| Package        | Description                   |
| -------------- | ----------------------------- |
| base           | Minimal Arch base system      |
| base-devel     | Build tools (gcc, make, etc.) |
| linux          | Linux kernel                  |
| linux-firmware | Firmware blobs                |
| intel-ucode    | Intel CPU microcode updates   |
| lvm2           | Logical Volume Manager        |
| networkmanager | Network management            |
| python         | Python interpreter            |
| openssh        | SSH client/server             |

#### Apps – base role (`apps/base`)

| Package                     | Description                          |
| --------------------------- | ------------------------------------ |
| gnu-free-fonts              | Free font family                     |
| noto-fonts                  | Google Noto (broad Unicode coverage) |
| noto-fonts-emoji            | Noto emoji font                      |
| ttf-nerd-fonts-symbols-mono | Nerd Font symbols (monospace)        |
| ttf-sourcecodepro-nerd      | Source Code Pro with Nerd Font icons |
| breeze-icons                | KDE Breeze icons (Qt apps fallback)  |
| papirus-icon-theme          | Modern SVG icon theme                |
| dunst                       | Notification daemon                  |
| rofi                        | Application launcher / menu          |
| rofi-calc                   | Calculator plugin for Rofi           |
| rofi-emoji                  | Emoji picker for Rofi                |
| picom                       | X compositor (transparency, shadows) |
| numlockx                    | Enable NumLock on startup            |
| xorg-xmodmap                | Keyboard remapping                   |
| arandr                      | Visual front end for XRandR          |
| brightnessctl               | Backlight/LED brightness control     |
| gvfs                        | Virtual filesystem (mounts, trash)   |
| udisks2                     | Automount removable drives           |
| ntfs-3g                     | NTFS read/write support              |
| xdg-desktop-portal          | Portal service for sandboxed apps    |
| xdg-desktop-portal-gtk      | GTK backend for desktop portals      |
| xdg-utils                   | xdg-open / xdg-mime                  |
| shared-mime-info            | MIME type database                   |
| desktop-file-utils          | Desktop entry database               |
| firefox                     | Web browser                          |
| mpv                         | Video player                         |
| wezterm                     | GPU-accelerated terminal emulator    |
| btop                        | System resource monitor              |
| feh                         | Image viewer / wallpaper setter      |
| fzf                         | Fuzzy finder                         |
| mkcert                      | Local TLS certificates               |
| ncdu                        | Disk usage analyzer                  |
| nvm                         | Node version manager                 |
| pass                        | CLI password store (GPG)             |
| tig                         | Git TUI client                       |
| wget                        | File downloader                      |
| maim                        | Screenshot utility                   |
| slop                        | Region selection (used by maim)      |
| xclip                       | Clipboard CLI tool                   |
| gnome-keyring               | GNOME keyring                        |
| libsecret                   | Secret Service client library        |
| openconnect                 | Cisco/Juniper VPN client             |
| smbclient                   | SMB/CIFS client                      |
| wireguard-tools             | WireGuard VPN                        |
| postgresql-libs             | PostgreSQL client libraries          |

#### Apps – audio role (`apps/audio`)

| Package        | Description            |
| -------------- | ---------------------- |
| pipewire       | Audio/video server     |
| pipewire-alsa  | ALSA compatibility     |
| pipewire-pulse | PulseAudio replacement |
| wireplumber    | Session manager        |
| pavucontrol    | GUI volume mixer       |

#### Apps – bluetooth role (`apps/bluetooth`)

| Package     | Description                        |
| ----------- | ---------------------------------- |
| bluez       | Bluetooth protocol stack           |
| bluez-utils | Bluetooth CLI tools (bluetoothctl) |
| blueman     | Graphical Bluetooth manager        |

#### Apps – docker role (`apps/docker`)

| Package        | Description                   |
| -------------- | ----------------------------- |
| docker         | Container runtime             |
| docker-compose | Multi-container orchestration |

#### Apps – qtile role (`apps/qtile`)

| Package       | Description                          |
| ------------- | ------------------------------------ |
| qtile         | Tiling window manager (Python)       |
| xorg          | Xorg display server (group)          |
| xorg-xinit    | startx / xinit                       |
| python-iwlib  | Python wireless bindings (for Qtile) |
| python-psutil | Python system utilities              |

#### Apps – thunar role (`apps/thunar`)

| Package           | Description                |
| ----------------- | -------------------------- |
| thunar            | Lightweight file manager   |
| thunar-volman     | Removable media management |
| tumbler           | Thumbnail service          |
| ffmpegthumbnailer | Video thumbnails           |
| gvfs-smb          | SMB/CIFS for gvfs          |

#### Apps – vim role (`apps/vim`)

| Package | Description |
| ------- | ----------- |
| vim     | Text editor |

#### Apps – zsh role (`apps/zsh`)

| Package | Description |
| ------- | ----------- |
| zsh     | Z-Shell     |

#### Apps – yazi role (`apps/yazi`)

| Package     | Description                |
| ----------- | -------------------------- |
| yazi        | Terminal file manager      |
| ffmpeg      | Multimedia framework       |
| 7zip        | 7z archive tool            |
| jq          | JSON processor             |
| poppler     | PDF rendering library      |
| fd          | Fast find alternative      |
| ripgrep     | Fast grep (rg)             |
| zoxide      | Smart cd (remembers paths) |
| imagemagick | Image manipulation (CLI)   |

#### Apps – xsecurelock role (`apps/xsecurelock`)

| Package     | Description          |
| ----------- | -------------------- |
| xsecurelock | Secure screen locker |
| xss-lock    | Screen lock on idle  |

#### Other (setup role)

| Package | Description     |
| ------- | --------------- |
| git     | Version control |

> **Package installer:** All packages are installed with `pacman` (official repos
> only). AUR packages are not installed automatically — see
> [Optional AUR packages](#optional-aur-packages) below.

---

### Not yet integrated – recommended for Ansible

These packages are missing from the current roles and should be added:

#### System / Hardware (add to `base_packages` or new role)

| Package            | Description                             | Notes                            |
| ------------------ | --------------------------------------- | -------------------------------- |
| zram-generator     | Swap-on-ZRAM configuration              | Add to bootstrap                 |
| systemd-resolvconf | systemd-resolved compatibility          | Add to bootstrap                 |
| intel-media-driver | Intel VA-API driver (HW video decoding) | New `hardware` role or bootstrap |
| vulkan-intel       | Intel Vulkan driver                     | Same                             |
| xf86-video-vesa    | Generic VESA fallback graphics driver   | Same                             |
| usbutils           | USB diagnostics (lsusb)                 | Same                             |

#### Network (add to `apps/base` or new `network` role)

| Package    | Description                             |
| ---------- | --------------------------------------- |
| net-tools  | Classic network tools (ifconfig, route) |
| bind       | DNS utilities (dig, nslookup)           |
| iperf3     | Network bandwidth measurement           |
| traceroute | Network route tracing                   |
| tcpdump    | Network packet analysis                 |

#### Audio (add to `apps/audio`)

| Package       | Description              |
| ------------- | ------------------------ |
| alsa-utils    | ALSA soundcard utilities |
| pipewire-jack | JACK compatibility       |
| pamixer       | CLI volume control       |

#### Window Manager / Desktop (add to `apps/base` or `apps/qtile`)

| Package            | Description                      |
| ------------------ | -------------------------------- |
| rofi-bluetooth-git | Bluetooth menu for Rofi (AUR)    |
| autorandr          | Automatic monitor profiles       |
| xdotool            | X11 automation (window/keyboard) |
| gnome-screenshot   | GNOME screenshot tool            |
| xorg-xrandr        | Display configuration            |
| xorg-xev           | X event display (debugging)      |
| xorg-xset          | X settings                       |
| xorg-xsetroot      | Root window background           |
| xorg-xhost         | X server access control          |
| xorg-xkill         | Kill window by click             |
| xorg-xbacklight    | Screen brightness (Xorg)         |
| xorg-xauth         | X authentication                 |
| xorg-xinput        | Input device configuration       |
| xorg-xdpyinfo      | Display info                     |

#### Terminal / Shell (add to `apps/base` or new role)

| Package    | Description                    |
| ---------- | ------------------------------ |
| tmux       | Terminal multiplexer           |
| neovim     | Modern Vim fork                |
| neofetch   | System info display            |
| pv         | Pipe viewer (progress display) |
| ueberzugpp | Image display in terminal      |

#### Development (add to `apps/base` or new `dev` role)

| Package              | Description                            |
| -------------------- | -------------------------------------- |
| ddev-bin             | Docker-based dev environment (PHP/Web) |
| pnpm                 | Fast Node.js package manager           |
| yarn                 | Node.js package manager                |
| mkcert               | Local TLS certificates                 |
| gtk-doc              | GTK documentation generator            |
| python-dbus-fast     | Fast Python D-Bus bindings             |
| kiro-ide             | AI-powered IDE (AUR)                   |
| beekeeper-studio-bin | SQL database client (GUI, AUR)         |
| postman-bin          | API testing tool (AUR)                 |

#### Browser / Communication (add to `apps/base`)

| Package             | Description                         |
| ------------------- | ----------------------------------- |
| teams-for-linux-bin | Microsoft Teams client (AUR)        |
| obsidian            | Markdown knowledge management (AUR) |

#### Media / Graphics (add to `apps/base` or new `media` role)

| Package      | Description                    |
| ------------ | ------------------------------ |
| hypnotix     | IPTV player (AUR)              |
| plex-desktop | Plex media player (AUR)        |
| audacity     | Audio editor                   |
| handbrake    | Video transcoder               |
| dvdbackup    | DVD backup tool                |
| libheif      | HEIF/HEIC image format support |
| libavif      | AVIF image format support      |
| ghostscript  | PostScript/PDF interpreter     |
| nsxiv        | Lightweight image viewer       |
| guvcview     | Webcam application             |

#### Remote / VPN / File Transfer (add to `apps/base` or new role)

| Package       | Description                             |
| ------------- | --------------------------------------- |
| remmina       | Remote desktop client (RDP/VNC)         |
| freerdp       | RDP implementation                      |
| localsend-bin | Local file transfer (AirDrop-like, AUR) |
| rsync         | File synchronization                    |

#### Printing / Scanning (new `printing` role recommended)

| Package     | Description      |
| ----------- | ---------------- |
| cups        | Print system     |
| simple-scan | Scan application |

#### System Tools (add to `apps/base` or new role)

| Package    | Description                       |
| ---------- | --------------------------------- |
| timeshift  | System backup/snapshots           |
| unzip      | ZIP extraction                    |
| unrar      | RAR extraction                    |
| 7zip       | 7z archive tool                   |
| ansible    | IT automation                     |
| solaar     | Logitech device manager (AUR)     |
| rpi-imager | Raspberry Pi SD card imager (AUR) |

#### Gaming

| Package | Description     | Notes                                    |
| ------- | --------------- | ---------------------------------------- |
| steam   | Gaming platform | Requires `[multilib]` repo enabled first |

---

### Manual installation (not suitable for standard Ansible automation)

| Package              | Reason                                                                                    |
| -------------------- | ----------------------------------------------------------------------------------------- |
| brother-mfc-l2860dwe | Proprietary AUR driver, needs manual post-install config (IP/USB setup via `brprintconf`) |
| brscan5              | Brother scanner driver, requires `brsaneconfig5` post-install setup                       |

> These can technically be automated with extra Ansible tasks (shell module + handlers), but typically require manual verification after installation.

---

### Recommended installation order

1. **Arch bootstrap** (`pacstrap`): `base`, `linux`, `linux-firmware`, `intel-ucode`, `base-devel`
2. **Ansible bootstrap tag**: Basic config (locale, timezone, user, bootloader, network)
3. **Enable multilib** (if Steam is needed): Edit `/etc/pacman.conf`
4. **Ansible main setup tag**: All pacman packages (official repos only)
5. **Optional AUR packages**: Install manually with an AUR helper (see below)
6. **Manual post-install**: Brother printer/scanner config, verify Steam/gaming
