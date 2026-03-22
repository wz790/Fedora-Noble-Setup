# ✨ My Fedora Linux Noble Setup Guide (Post-Installation)

<p align="center">
  <img src="src/assets/logo.png" alt="Fedora Setup Logo - Nord Style" width="250"/>
</p>

<div align="center">

[![Fedora](https://img.shields.io/badge/Fedora-43-blue.svg?style=for-the-badge&logo=fedora)](https://getfedora.org/)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-brightgreen.svg?style=for-the-badge)](https://github.com/yourusername/fedora-setup/graphs/commit-activity)

</div>

---

## 👋 Hey there!

Since you're here, chances are you've just installed Fedora and now you're looking at a new desktop wondering, "What next?" Fedora may require some configuration after a fresh install, which is why I've put together this guide, containing the basics and everything I've gathered from various sources.

Note: This guide is not an official guide. It's just a sharing of what I used, and some personal settings or preferences that you can skip. Just read and understand what you're doing before applying anything, and don't just copy and paste everything.

### 💡 Quick heads up about commands:

- When you see `sudo` at the start, that means "run as admin" it'll ask for your password
- The `-y` flag just means "yes to everything" so you don't have to keep pressing enter
- Copy-paste is your friend, but **always read what you're about to run first**

---

## 📋 Table of Contents

<details>
<summary>Click to expand full guide</summary>

- [First Things](#first-things)
  - [RPM Fusion](#rpm-fusion)
  - [Updates](#updates)
  - [Firmware Updates](#firmware-updates)
  - [Give Your Computer a Name](#give-your-computer-a-name)
- [Getting More Software](#getting-more-software)
  - [Flathub Setup](#flathub-setup)
- [Graphics Drivers](#graphics-drivers)
  - [NVIDIA](#nvidia)
  - [AMD & Intel](#amd--intel)
- [Making Media Work](#making-media-work)
  - [Video Codecs](#video-codecs-so-everything-plays)
  - [Hardware Acceleration](#hardware-acceleration)
  - [Firefox Video Fix](#firefox-video-fix)
- [Useful Stuff](#useful-stuff)
  - [Archive Support](#archive-support)
  - [AppImage Support](#appimage-support)
  - [Dual Boot Time Fix](#dual-boot-time-fix)
- [Backup Your Stuff](#backup-your-stuff)
  - [System Snapshots](#system-snapshots)
  - [Personal Files](#personal-files)
- [Gaming Setup](#-gaming-setup)
  - [Steam and Gaming](#steam-and-gaming)
  - [Lutris](#lutris)
  - [MangoHud](#mangohud)
- [Apps I Actually Use](#-apps-i-actually-use)
  - [Browsers](#browsers)
  - [Development](#development)
    - [VS Code](#vs-code)
    - [VS Codium](#vs-codium)
    - [Sublime Text](#sublime-text)
    - [Git](#git)
    - [Seahorse](#seahorse)
  - [Python Tooling](#python-tooling)
    - [uv](#uv)
    - [ruff](#ruff)
  - [Node.js Tooling](#nodejs-tooling)
    - [NVM](#nvm)
  - [Containers](#containers)
  - [Multimedia](#multimedia)
    - [VLC](#vlc)
    - [MPV](#mpv)
    - [yt-dlp](#yt-dlp)
    - [OBS Studio](#obs-studio)
    - [Audacity](#audacity)
  - [Downloading & Syncing](#downloading--syncing)
    - [rclone](#rclone)
    - [Transmission](#transmission)
  - [Office Work](#office-work)
    - [OnlyOffice](#onlyoffice)
  - [Reading](#reading)
    - [Foliate](#foliate)
  - [System Tools](#system-tools)
    - [Mission Center](#mission-center)
    - [Flatseal](#flatseal)
    - [Warehouse](#warehouse)
    - [sshfs](#sshfs)
- [Desktop Environment](#️-desktop-environment)
  - [XFCE](#xfce)
    - [PCManFM](#pcmanfm)
    - [Redshift](#redshift)
    - [Conky](#conky)
    - [libinput-gestures](#libinput-gestures)
  - [GNOME](#gnome)
    - [Get GNOME Tweaks](#get-gnome-tweaks)
    - [Extra themes (if you're into that)](#extra-themes-if-youre-into-that)
    - [Extension Manager](#extension-manager)
    - [Terminal Transparency](#terminal-transparency)
    - [My must-have extensions](#my-must-have-extensions)
  - [KDE Plasma](#kde-plasma)
    - [Latte Dock](#latte-dock)
- [Keeping Things Clean](#-keeping-things-clean)
  - [System Cleanup](#system-cleanup)
- [⚡ Power Management & Battery](#-power-management--battery)
  - [TLP](#tlp)
</details>

---

## First Things

### RPM Fusion

Okay, first thing Fedora ships pretty bare-bones because of legal reasons. [RPM](https://rpmfusion.org/) Fusion is where all the actually useful stuff lives (codecs, drivers, etc.).

```bash
# Get the free repository
sudo dnf install -y \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# Get the nonfree repository (NVIDIA drivers, some codecs)
sudo dnf install -y \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

# Install AppStream metadata
sudo dnf install rpmfusion-free-appstream-data rpmfusion-nonfree-appstream-data
sudo dnf check-update
```

---

### Updates

We need to update because fresh installs always have outdated packages.

```bash
# Update everything
sudo dnf update -y
```
```bash
# It is recommended to restart after updating.
sudo reboot
```

> 💡 **Tip**: Make a habit of running `sudo dnf update` weekly or monthly :).

---

### Firmware Updates

Your hardware probably has newer firmware available. This actually matters for things like WiFi and battery life.

```bash
# See what can be updated
sudo fwupdmgr get-devices

# Refresh the firmware database
sudo fwupdmgr refresh --force

# Check for updates
sudo fwupdmgr get-updates

# Apply them
sudo fwupdmgr update
```

> ⚠️ **Note**: After firmware updates you need a reboot.

---

### Give Your Computer a Name

This is purely cosmetic but makes you feel more at home :)

```bash
# Replace with whatever you want
sudo hostnamectl set-hostname Pizza
```

---

## Getting More Software

### Flathub Setup

Fedora comes with a neutered version of [Flatpak](https://flatpak.org/). [Flathub](https://flathub.org/) is where the actual apps are.

```bash
# Remove the limited Fedora repo to prevent conflicts
flatpak remote-delete fedora
```
Here, I will leave the decision up to you; there are several options. Choose the one that suits you best. Here is a resource that may help you [flathub](https://docs.flathub.org/docs/for-users/installation#remove-subsets).

#### Option 1: Full Repository:

Get access to everything Flathub (that include apps that are not officially maintained by their developers):

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Option 2: Verified Apps Only:

Only apps maintained by their actual developers :

```bash
flatpak remote-add --if-not-exists --subset=verified flathub https://flathub.org/repo/flathub.flatpakrepo
```

**Note:** Some popular apps won't show up because they're packaged by the community.

#### Option 3: Open Source Only:

This for only free and open source software :

```bash
flatpak remote-add --if-not-exists --subset=floss flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Option 4: Verified + Open Source:

The most restrictive option it include only apps that are both verified by developers and open source :

```bash
flatpak remote-add --if-not-exists --subset=verified_floss flathub https://flathub.org/repo/flathub.flatpakrepo
```
---

## Graphics Drivers

### NVIDIA

NVIDIA drivers on Fedora are best installed via RPM Fusion's akmod-nvidia packages, there are two options for you first with enabled Secure Boot (preferred) or disable it. The choice is yours.
The best official source is here [RPMFUSION HOW TO NVIDIA](https://rpmfusion.org/Howto/NVIDIA) for you to look into yourself, while I will try to be brief.

#### Prerequisites (both options)
Install kernel headers and dev tools:
```bash
sudo dnf install kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig
```
For RTX 4000+ GPUs, set open kernel module:
```bash
sudo sh -c 'echo "%_with_kmod_nvidia_open 1" > /etc/rpm/macros.nvidia-kmod'
```

#### Option 1: Secure Boot Enabled
Install signing tools before driver:
```bash
sudo dnf install akmods mokutil openssl
sudo kmodgenca -a
sudo mokutil --import /etc/pki/akmods/certs/public_key.der
```
Reboot, enroll MOK key in blue screen (set password, confirm), then reboot again.

Install driver:
```bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```
Wait 5-15 min for build, you can monitor progress with:(`sudo journalctl -f -u akmods`).
When finished, reboot.

#### Option 2: Secure Boot Disabled
Disable Secure Boot in BIOS/UEFI settings first.

Then install directly:
```bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```
Wait for build, then reboot.

#### Verification After installation
To verify, the following commands will give you the status of the driver and secure boot.
```bash
nvidia-smi
modinfo nvidia
mokutil --sb-state
```
<details>
<summary>🆘 Troubleshooting stuck at 800×600 / black screen / terminal!</summary>

The NVIDIA module probably didn't build correctly. Here's what to do:

1. **Black screen/low res: Boot into an older kernel**:
   - At GRUB menu, select "Advanced Options"
   - Pick the previous kernel version

2. **Build fails: Force rebuild**:
   ```bash
   sudo akmods --rebuild --force
   sudo reboot
   ```

3. **Still broken?** Check the logs:
   ```bash
   sudo journalctl -u akmods
   dmesg | grep -i nvidia
   ```

</details>

> 💡 **Tip**: After kernel updates, you might need to manually do:
>
> ```bash
> sudo akmods --force --kernels $(uname -r)
> sudo reboot
> ```

---

### AMD & Intel

These usually just work, but let's make them work **better**.

#### Core Packages (AMD & Intel):

Vulkan and basic acceleration:
```bash
sudo dnf install mesa-vulkan-drivers vulkan-loader mesa-libGLU libva-utils
```

#### AMD (Video Acceleration):

Replace defaults with freeworld for full codec support (H.264/HEVC):
```bash
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
```

#### Intel (Video Acceleration)

* Intel Newer GPUs (11th Gen+):

```bash
# Intel video acceleration (for newer Intel GPUs)
sudo dnf install -y intel-media-driver
```

* Intel Older GPUs (pre-11th Gen):

```bash
# Intel video acceleration (for Grandfather Intel GPUs)
sudo dnf install -y libva-intel-driver
```
---

## Making Media Work

### Video Codecs

Fedora ships with basically no codecs because of patent issues, so RPM Fusion is required for full H.264, HEVC, VP9, and AV1 support. You can follow the official RPM Fusion guide [RPM FUSION How to Multimedia](https://rpmfusion.org/Howto/Multimedia/) to understand more, but I will summarise it for you.

Swap to full FFmpeg (essential for most apps(
```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```
Update multimedia group (GStreamer full + codecs)

```bash
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```
Optional sound/video group (apps/plugins)

```bash
sudo dnf groupupdate sound-and-video
```

---

### Hardware Acceleration

This makes video playback use your GPU instead of hammering your CPU.

```bash
# Install VA-API stuff
sudo dnf install -y ffmpeg-libs libva libva-utils
```

**NVIDIA users, add this too:**

```bash
sudo dnf install -y libva-nvidia-driver
```

---

### Firefox Video Fix

Firefox needs a little help with H.264 videos.

Install the Cisco codec and enable it:
```bash
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264

sudo dnf config-manager --set-enabled fedora-cisco-openh264
sudo dnf update -y
```

> ⚠️ **Important**: Restart Firefox and check that the OpenH264 plugin is enabled in `about:addons`.

---

## Useful Stuff

### Archive Support

Because you'll definitely need to extract a `.rar` file someday.

```bash
sudo dnf install -y p7zip p7zip-plugins unrar
```

---

### AppImage Support

Some apps only come as AppImages. This makes them work.

```bash
# Install FUSE
sudo dnf install -y fuse fuse-libs
```
Optional: AppImage manager (actually pretty useful)
```bash
flatpak install -y flathub it.mijorus.gearlever
```

---

### Dual Boot Time Fix

If you dual boot with Windows, this fixes the clock being wrong.

Tell Fedora to use UTC for hardware clock
```bash
sudo timedatectl set-local-rtc 0 --adjust-system-clock
```

---

## Backup Your Stuff

### System Snapshots

If you're using Btrfs (default in Fedora), you can take snapshots with btrfs assistant.

```bash
sudo dnf install -y btrfs-assistant btrbk snapper
```

#### Setup:

Run `btrfs-assistant` to set up snapshots with a GUI

#### Enable automatic snapshots:

```bash
sudo systemctl enable --now snapper-timeline.timer
sudo systemctl enable --now snapper-cleanup.timer
```

> ⚠️ **Important**: This only backs up system files, not your personal stuff unless you configure it differently.

---

### Personal Files

Use Déjà Dup for your documents, photos, etc.

Install it From fedora repos :
```bash
sudo dnf install -y deja-dup
```

Or get it from Flathub :
```bash
flatpak install -y flathub org.gnome.DejaDup
```

> 💡 **Tip**: Always backup to external storage or cloud, not the same disk.

---

## Gaming Setup

### Steam and Gaming

Install [Steam](https://store.steampowered.com/) from fedora repos :

```bash
sudo dnf install -y steam
```

> 📌 **Note**: If you're using a newer NVIDIA GPU and Steam doesn't launch or acts weird, it's probably a missing dependency issue. Remove the RPM version and install the Flatpak instead with :

```bash
sudo dnf remove steam

flatpak install -y flathub com.valvesoftware.Steam
```
### Lutris

[Lutris](https://lutris.net/) is perfect for managing non-Steam games, emulators, and older titles.

```bash
sudo dnf install -y lutris
```

Or get the Flatpak version:

```bash
flatpak install -y flathub net.lutris.Lutris
```

### MangoHud

[MangoHud](https://github.com/flightlessmango/MangoHud) is an overlay that shows FPS, CPU/GPU temps, and more.

```bash
sudo dnf install -y mangohud
```

---

## 📦 Apps I Actually Use

### Browsers

#### Brave

[Brave](https://brave.com/) blocks ads, pretty fast:

```bash
sudo dnf install dnf-plugins-core
sudo dnf config-manager addrepo --from-repofile=https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo
sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc
sudo dnf install -y brave-browser
```

#### LibreWolf

[LibreWolf](https://librewolf.net/) is privacy-focused Firefox fork:

```bash
# Add the repo
curl -fsSL https://repo.librewolf.net/librewolf.repo | pkexec tee /etc/yum.repos.d/librewolf.repo

# Install the package
sudo dnf install -y librewolf
```

---

### Development

#### VS Code

[Visual Studio Code](https://code.visualstudio.com/) is basically Microsoft telemetry central, but I still use it anyway. If privacy matters, check out VSCodium instead.

```bash
# Import Microsoft GPG key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Add VS Code repository
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'

# Install VS Code
sudo dnf install -y code
```

#### VS Codium

[VSCodium](https://vscodium.com) is a community-driven, freely-licensed binary distribution of Microsoft's editor VS Code.

```bash
# Add the GPG key of the repository:
sudo rpmkeys --import https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg

# Add VSCodium repository
printf "[gitlab.com_paulcarroty_vscodium_repo]\nname=download.vscodium.com\nbaseurl=https://download.vscodium.com/rpms/\nenabled=1\ngpgcheck=1\nrepo_gpgcheck=1\ngpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg\nmetadata_expire=1h\n" | sudo tee -a /etc/yum.repos.d/vscodium.repo

# Install VS Codium
sudo dnf install codium
```

#### Sublime Text

[Sublime Text](https://www.sublimetext.com/) is fast, lightweight, and doesn't ask you to install an extension for everything. Great for quick edits and writing.

```bash
# Add the Sublime Text repo
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

sudo dnf config-manager addrepo --from-repofile=https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

# Install
sudo dnf install -y sublime-text
```

#### Git

[Git](https://git-scm.com/) for version control — you'll need this for basically everything.

```bash
sudo dnf install -y git
```

#### Seahorse

[Seahorse](https://wiki.gnome.org/Apps/Seahorse) is the GNOME keyring manager. Fixes the "Failed to unlock the keyring" or password prompt loop you get with **VS Code** and **Brave** on non-GNOME desktops like XFCE.

After installing, launch it once, set a blank password on the `Login` keyring, and those annoying popups go away permanently.

```bash
sudo dnf install -y seahorse
```

---

### Python Tooling

#### uv

[uv](https://github.com/astral-sh/uv) is a blazing-fast Python package and project manager written in Rust. It replaces `pip`, `venv`, `pyenv`, and `pip-tools` in one tool. I use it for everything Python-related now.

```bash
# Install via the official script
curl -LsSf https://astral.sh/uv/install.sh | sh
```

This installs the `uv` binary to `~/.cargo/bin/`. Make sure that's in your PATH:

```bash
export PATH="$HOME/.cargo/bin:$PATH"
```

> 💡 **Tip**: Add the export to your `~/.bashrc` or `~/.bash_aliases` so it persists across sessions.

Common usage:

```bash
uv init my-project       # Create a new project
uv add requests          # Add a dependency
uv run script.py         # Run a script in the project environment
uv venv                  # Create a virtual environment
uv pip install ...       # Drop-in pip replacement
```

#### ruff

[ruff](https://github.com/astral-sh/ruff) is an extremely fast Python linter and code formatter, also from Astral (same folks as uv).

```bash
uv tool install ruff
```

---

### Node.js Tooling

#### NVM

[NVM](https://github.com/nvm-sh/nvm) (Node Version Manager) lets you install and switch between multiple Node.js versions without messing up your system.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

Then add this to your `~/.bashrc` or `~/.bash_aliases`:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

Install and use a Node version:

```bash
nvm install --lts        # Install the latest LTS
nvm use --lts            # Use it
nvm alias default lts/*  # Make it the default
```

Set a global npm prefix so global packages don't need sudo:

```bash
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=$HOME/.npm-global/bin:$PATH
```

---

### Containers

Podman is better than Docker, fight me :)

```bash
# Podman core
sudo dnf install -y podman podman-compose

# Podman GUI
flatpak install -y flathub io.podman_desktop.PodmanDesktop
```

> 💡 **Note**: If you need Docker compatibility for specific tools, you can install Docker CE as an alternative:
> ```bash
> sudo dnf install -y dnf-utils
> sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
> sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
> sudo usermod -aG docker $USER
> sudo systemctl enable --now docker
> ```
> Log out and back in for group changes to take effect.

---

### Multimedia

[VLC](https://www.videolan.org/vlc/) is my go-to multimedia player, but it's totally up to you whether you use it or choose something else:

```bash
# Install VLC
sudo dnf install -y vlc
```

#### MPV

[MPV](https://mpv.io/) is a lightweight, powerful media player with minimal UI and excellent codec support:

```bash
sudo dnf install -y mpv
```

```bash
mpv video.mp4
mpv https://youtu.be/...   # works with yt-dlp under the hood
```

#### yt-dlp

[yt-dlp](https://github.com/yt-dlp/yt-dlp) is a YouTube (and basically everything else) downloader. The actively maintained fork of youtube-dl with more features and faster updates.

```bash
uv tool install yt-dlp
```

Basic usage:

```bash
yt-dlp <url>                            # Download video
yt-dlp -f bestaudio --extract-audio <url>  # Audio only
yt-dlp -o "%(title)s.%(ext)s" <url>    # Custom filename
```

#### OBS Studio

[OBS Studio](https://obsproject.com/) for screen recording and streaming:

```bash
flatpak install -y flathub com.obsproject.Studio
```

#### Audacity

[Audacity](https://www.audacityteam.org/) for audio editing:

```bash
flatpak install -y flathub org.audacityteam.Audacity
```

---

### Downloading & Syncing

#### rclone

[rclone](https://rclone.org/) is like rsync but for cloud storage. I use it to sync files between my machine, Google Drive, and other remotes. It supports 70+ storage providers.

```bash
# Install via official script (always gets the latest version)
curl https://rclone.org/install.sh | sudo bash
```

Configure your remotes:

```bash
rclone config   # Interactive setup wizard
```

Common usage:

```bash
rclone ls remote:path              # List files
rclone copy local/path remote:path # Copy to remote
rclone sync local/path remote:path # Sync (one-way)
rclone mount remote: ~/Remotes/gdrive --daemon  # Mount as filesystem
```

> 💡 **Tip**: Add an alias to keep rclone updated:
> ```bash
> alias upd-rclone='sudo -v ; curl https://rclone.org/install.sh | sudo bash'
> ```

#### Transmission

[Transmission](https://transmissionbt.com/) is a lightweight, no-nonsense BitTorrent client. Low on resources and gets out of your way.

```bash
sudo dnf install -y transmission
```

> 📌 **Note**: This installs the GTK version. If you want a web interface instead (useful for headless or remote use):
> ```bash
> sudo dnf install -y transmission-daemon
> ```

---

### Office Work

[OnlyOffice](https://www.onlyoffice.com/) for better MS Office compatibility:

```bash
flatpak install -y flathub org.onlyoffice.desktopeditors
```

---

### Reading

#### Foliate

[Foliate](https://johnfactotum.github.io/foliate/) is a clean, minimal ebook reader that supports EPUB, MOBI, FB2, CBZ, and PDF. Perfect if you read a lot of tech books or ebooks.

```bash
flatpak install -y flathub com.github.johnfactotum.Foliate
```

---

### System Tools

#### Mission Center

[Mission Center](https://missioncenter.io/) is a cool system monitor:

```bash
flatpak install flathub io.missioncenter.MissionCenter
```

#### Flatseal

[Flatseal](https://flathub.org/apps/details/com.github.tchx84.Flatseal) manages Flatpak permissions:

```bash
flatpak install -y flathub com.github.tchx84.Flatseal
```

#### Warehouse

Warehouse provides a simple UI to control complex Flatpak options, all without resorting to the command line.

```bash
flatpak install flathub io.github.flattool.Warehouse
```

#### sshfs

Mount remote directories over SSH as if they were local folders. I use this to mount my Termux storage from my Android phone.

```bash
sudo dnf install -y sshfs
```

Usage:

```bash
# Mount
sshfs user@host:/remote/path ~/Remotes/mountpoint -o follow_symlinks

# Unmount
fusermount -u ~/Remotes/mountpoint
```

---

## 🖥️ Desktop Environment

### XFCE

XFCE is fast, lightweight, and stays out of your way — perfect for older hardware like the ThinkPad T480.

#### PCManFM

[PCManFM](https://wiki.lxde.org/en/PCManFM) is a snappy, no-bloat file manager. Handles network mounts and MTP devices without complaint.

```bash
sudo dnf install -y pcmanfm
```

> 💡 **Tip**: Set PCManFM as your default file manager in XFCE Settings > Default Applications > Utilities.

#### Redshift

[Redshift](http://jonls.dk/redshift/) adjusts your screen's color temperature based on the time of day. Warm tones at night, normal during the day — your eyes will thank you.

```bash
sudo dnf install -y redshift
```

For XFCE, use the GTK frontend so you get a tray icon:

```bash
sudo dnf install -y redshift-gtk
```

Auto-start it with your session:

```bash
# Add to XFCE Session > Application Autostart
redshift-gtk
```

Or create a minimal config at `~/.config/redshift.conf`:

```ini
[redshift]
temp-day=5500
temp-night=3500
fade=1
location-provider=manual

[manual]
lat=14.6  ; adjust to your location
lon=121.0
```

#### Conky

[Conky](https://github.com/brndnmtthws/conky) is a lightweight system monitor that renders directly on your desktop. Highly customizable — CPU, RAM, disk, network, and whatever else you want.

```bash
sudo dnf install -y conky
```

Create a basic config at `~/.conkyrc`:

```lua
conky.config = {
    update_interval = 1,
    background = true,
    alignment = 'top_right',
    gap_x = 15,
    gap_y = 45,
    own_window = true,
    own_window_type = 'desktop',
    own_window_transparent = true,
    own_window_hints = 'undecorated,below,sticky,skip_taskbar,skip_pager',
    double_buffer = true,
    draw_shades = false,
    use_xft = true,
    font = 'Monospace:size=9',
    default_color = 'white',
    default_shade_color = 'black',
}

conky.text = [[
${color grey}CPU:$color ${cpu}% ${cpubar 8}
${color grey}RAM:$color $mem / $memmax ${membar 8}
${color grey}Disk:$color ${fs_used /} / ${fs_size /}
${color grey}Net Up:$color ${upspeed} | Down: ${downspeed}
${color grey}Uptime:$color $uptime
]]
```

Auto-start with XFCE Session > Application Autostart: `conky --daemonize --pause=3`

#### libinput-gestures

[libinput-gestures](https://github.com/bulletmark/libinput-gestures) lets you use touchpad gestures (swipes, pinches) to trigger actions like workspace switching, window management, or custom commands.

```bash
# Install dependencies
sudo dnf install -y libinput-tools xdotool

# Add your user to the input group
sudo gpasswd -a $USER input

# Install from source
git clone https://github.com/bulletmark/libinput-gestures.git
cd libinput-gestures
sudo make install
```

> ⚠️ **Note**: You need to log out and log back in (or reboot) after adding yourself to the `input` group for permissions to take effect.

Configure custom gestures in `~/.config/libinput-gestures.conf`:

```bash
# Example: 3-finger swipe up/down for volume
gesture swipe up 3 xdotool keyup volumeup
gesture swipe down 3 xdotool keyup volumedown
```

**xdotool** is used alongside libinput-gestures to simulate keyboard presses and mouse actions from your gesture bindings.

> 💡 **Tip**: Copy the example config to get started:
> ```bash
> cp /etc/libinput-gestures.conf ~/.config/libinput-gestures.conf
> ```

---

### GNOME

#### Get GNOME Tweaks

With it you can add minimize/maximize buttons and some gnome tweaks.

```bash
sudo dnf install -y gnome-tweaks
```

---

#### Extra themes (if you're into that)

I stick with default but some people like options. Up to you.

```bash
sudo dnf install -y gnome-themes-extra
```

---

#### Extension Manager

A utility for browsing and installing GNOME Shell Extensions.

```bash
flatpak install -y flathub com.mattjakeman.ExtensionManager
```

---

#### Terminal Transparency

It's just better with it for me:

```bash
gsettings set org.gnome.Ptyxis.Profile:/org/gnome/Ptyxis/Profiles/$PTYXIS_PROFILE/ opacity .90
```

---

#### My must-have extensions:

> 💡 **Disclaimer**: It's just my personal preference, do what you want :)

- **Vitals** - shows CPU/RAM usage in top bar, pretty handy
- **Blur my Shell** - makes everything look less flat and boring
- **Dash to Dock** - turns the dock into something actually useful

---

### KDE Plasma

#### Latte Dock

[Latte Dock](https://github.com/KDE/latte-dock) for a macOS-like experience:

```bash
sudo dnf install -y latte-dock
```

---

## 🧹 Keeping Things Clean

### System Cleanup

Clean package cache and remove orphaned packages

```bash
sudo dnf clean all

sudo dnf autoremove -y
```

Useful cleanup aliases:

```bash
alias upd='sudo dnf upgrade'
alias upd-full='sudo dnf upgrade --refresh'
alias clean-leaves='sudo dnf autoremove'
alias clean-cache='sudo dnf clean packages'
alias clean-all='sudo dnf clean all'
alias upkeep='upd && clean-leaves'
alias deep-clean='upd-full && clean-leaves && clean-all'
```

---

## ⚡ Power Management & Battery

### TLP

[TLP](https://linrunner.de/tlp/) is advanced power management for Linux laptops. It automatically optimizes power settings in the background, giving you better battery life without any manual tweaking.

```bash
# Install TLP
sudo dnf install -y tlp tlp-rdw

# Enable and start TLP
sudo systemctl enable --now tlp

# Apply settings immediately
sudo tlp start
```

That's it — TLP works with sensible defaults. If you want to customize:

```bash
# Edit the config file
sudo nano /etc/tlp.conf
```

**Useful settings to tweak:**

| Setting | AC (Plugged In) | Battery | What it does |
|---------|-----------------|---------|--------------|
| `CPU_SCALING_GOVERNOR` | `performance` | `powersave` | CPU frequency scaling |
| `WIFI_PWR_ON_AC` | `off` | `on` | WiFi power saving |
| `RADEON_POWER_PROFILE` | `high` | `low` | AMD GPU power profile |
| `THINKPAD_CHARGE_THRESHOLDS` | `90 100` | `40 80` | Battery charge limits (ThinkPad only) |

**ThinkPad battery health tip:** Set charge thresholds to `40 80` on battery to extend battery lifespan:

```bash
# In /etc/tlp.conf, uncomment and set:
START_CHARGE_THRESH_BAT0=40
STOP_CHARGE_THRESH_BAT0=80
```

**Verification commands:**

```bash
# Check TLP status
sudo tlp-stat -s

# View current power settings
sudo tlp-stat -p

# Battery health and thresholds
sudo tlp-stat -b

# Full system report
sudo tlp-stat -a
```

> 💡 **Tip**: Install `powertop` for real-time power monitoring:
> ```bash
> sudo dnf install -y powertop
> sudo powertop          # Interactive monitor
> sudo powertop --auto-tune  # One-time tuning (TLP handles this automatically)
> ```

> ⚠️ **Note**: TLP conflicts with `power-profiles-daemon` (used by GNOME). TLP will automatically mask it during installation. If you want to use GNOME's power profiles instead, remove TLP:
> ```bash
> sudo dnf remove tlp
> ```

---

## 👏 Thanks

This guide exists because a bunch of people before me figured this stuff out and shared it. Big thanks to folks like **devangshekhawat**, **paulsorensen**, **Mohammed Besar**, and countless others on forums and Reddit who've helped people get Fedora working properly.

---

<div align="center">

### ⚠️ Disclaimer

This isn't official Fedora documentation it's just what works for me. Your mileage may vary. Always backup important stuff before making big changes, and don't blame me if something breaks (but feel free to ask for help on the [Fedora forums](https://discussion.fedoraproject.org/)).

---

**Enjoy your day :)**

</div>
