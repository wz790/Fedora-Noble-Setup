# ✨ My Fedora Linux Noble Setup Guide (Post-Installation)

<p align="center">
  <img src="src/assets/logo.png" alt="Fedora Setup Logo - Nord Style" width="250"/>
</p>

Hey there! So you just installed Fedora and now you're staring at a fresh desktop wondering "what next?" I've been there, and honestly, setting up a new Linux install can feel overwhelming. That's why I put together this guide - it's basically everything I wish someone had told me when I first switched to Fedora.

This isn't some corporate documentation. It's just me sharing what actually works, what breaks, and how to fix it when things go sideways (because they will, and that's totally normal).

**Who's this for?**
- Brand new to Fedora? Perfect.
- Coming from Ubuntu or another distro? Also perfect.
- Been using Fedora for years but want a solid checklist? Yep, you too.
- Complete Linux newbie? You might struggle a bit, but stick with it!

**Quick heads up about commands:**
- When you see `sudo` at the start, that means "run as admin" - it'll ask for your password
- The `-y` flag just means "yes to everything" so you don't have to keep pressing enter
- Copy-paste is your friend, but always read what you're about to run first

---

## 📋 What We're Covering

1. [🔥 First Things First](#-first-things-first)
   - [RPM Fusion - The Good Stuff](#rpm-fusion---the-good-stuff)
   - [Updates (Boring but Important)](#updates-boring-but-important)
   - [Firmware Updates](#firmware-updates)
   - [Give Your Computer a Name](#give-your-computer-a-name)
2. [📦 Getting More Software](#-getting-more-software)
   - [Flathub Setup](#flathub-setup)
   - [Terra Repository](#terra-repository-if-youre-feeling-adventurous)
3. [🎮 Graphics Drivers](#-graphics-drivers)
   - [NVIDIA (The Tricky One)](#nvidia-the-tricky-one)
   - [AMD & Intel (The Easy Ones)](#amd--intel-the-easy-ones)
4. [🎵 Making Media Work](#-making-media-work)
   - [Video Codecs](#video-codecs-so-everything-plays)
   - [Hardware Acceleration](#hardware-acceleration)
   - [Firefox Video Fix](#firefox-video-fix)
5. [🔧 Useful Stuff](#-useful-stuff)
   - [Archive Support](#archive-support)
   - [Microsoft Fonts](#microsoft-fonts-unfortunately-still-needed)
   - [AppImage Support](#appimage-support)
6. [⚡ Making Things Faster](#-making-things-faster)
   - [Faster Boots](#faster-boots)
   - [Better Battery Life](#better-battery-life)
   - [Dual Boot Time Fix](#dual-boot-time-fix)
   - [Performance Tweaks](#performance-tweaks-proceed-with-caution)
7. [🔒 Security Stuff](#-security-stuff)
   - [Encrypted DNS](#encrypted-dns-optional-but-cool)
8. [💾 Backup Your Stuff](#-backup-your-stuff)
   - [System Snapshots](#system-snapshots)
   - [Personal Files](#personal-files)
9. [🎮 Gaming Setup](#-gaming-setup)
   - [Steam and Gaming](#steam-and-gaming)
10. [🌟 Apps I Actually Use](#-apps-i-actually-use)
    - [Browsers](#browsers)
    - [Development](#development)
    - [Office Work](#office-work)
11. [🖥️ Desktop environment](#-desktop-environment)
    - [GNOME](#gnome)
    - [KDE](#kde)
12. [🧹 Keeping Things Clean](#-keeping-things-clean)
    - [Removing Bloat](#removing-bloat)
    - [System Cleanup](#system-cleanup)
13. [👏 Thanks](#thanks)

---

## 🔥 First Things First

### RPM Fusion - The Good Stuff

Okay, first thing - Fedora ships pretty bare-bones because of legal reasons. RPM Fusion is where all the actually useful stuff lives (codecs, drivers, etc.). You want this.

```bash
# Get the free repository (most stuff you need)
sudo dnf install -y \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# Get the nonfree repository (NVIDIA drivers, some codecs)
sudo dnf install -y \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

# Update everything so it all plays nice together
sudo dnf group upgrade core -y
sudo dnf check-update
```

### Updates (Boring but Important)

Yeah, I know, updates are boring. But seriously, do this first. Fresh installs always have outdated packages.

```bash
# Update everything
sudo dnf update -y

# If it updated the kernel, reboot
sudo reboot
```

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

Some firmware updates need a reboot. Just do it.

### Give Your Computer a Name

This is purely cosmetic but makes you feel more at home. Pick something fun!

```bash
# Replace with whatever you want
sudo hostnamectl set-hostname my-awesome-fedora-box
```

## 📦 Getting More Software

### Flathub Setup

Fedora comes with a neutered version of Flatpak. Flathub is where the actual apps are.

```bash
# Remove the limited Fedora repo
flatpak remote-delete fedora

# Add the real Flathub
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Update everything
flatpak update --appstream
```

### Terra Repository (If You're Feeling Adventurous)

Terra has some community packages that aren't in the main repos. It's optional but sometimes useful.

```bash
sudo dnf install -y --nogpgcheck --repofrompath 'terra,https://repos.fyralabs.com/terra$releasever' terra-release
```

## 🎮 Graphics Drivers

### NVIDIA (The Tricky One)

NVIDIA on Linux is... complicated. This works most of the time, but if you have issues, welcome to the club.

**Before you start:**
- Turn off Secure Boot in your BIOS (or learn to sign kernel modules – your choice)
- Make sure everything is updated and rebooted

```bash
# Update and reboot first
# Install kernel headers and dev tools
sudo dnf install -y kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig

# Enable RPM Fusion (if not already)
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

**Special Note for RTX 4000 and Newer Series:** If you're using a 4000 or 5000 series GPU (e.g. 4060, 4080, 5090), Fedora needs a build macro set before installing the driver. This enables the open kernel module.

```bash
# Set open kernel module macro (one-time step)
sudo sh -c 'echo "%_with_kmod_nvidia_open 1" > /etc/rpm/macros.nvidia-kmod'
```

Install the NVIDIA driver:
```bash
sudo dnf install -y akmod-nvidia xorg-x11-drv-nvidia-cuda
```

**Now wait.** Seriously. It takes 5–15 minutes to build the module.

You can monitor progress with:
```bash
sudo journalctl -f -u akmods
```

Once done:
```bash
sudo reboot
```

Check if it worked:
```bash
nvidia-smi
```

---

**If it doesn't work:**

```bash
# Force rebuild and try again
sudo akmods --force --kernels $(uname -r)
sudo reboot
```

**Extra Notes:**
- **Secure Boot:** The NVIDIA module isn't signed by default. Either disable Secure Boot or manually sign the module.
- **5000 Series (May 2025):** Fedora 42's live installer lacks proper Nouveau support for 5000 series GPUs. You might get stuck in 800×600 resolution during install, which can break the UI.
- **Manual rebuilds:** After kernel updates, especially on newer GPUs, you might need to run:
  ```bash
  sudo akmods --kernels $(uname -r) --rebuild
  sudo reboot
  ```

If you're stuck in 800×600, a black screen, or land in a terminal (tty) instead of your desktop, the NVIDIA module probably didn't build correctly. Use an older kernel from GRUB > Advanced Options and rerun the rebuild commands.


### AMD & Intel (The Easy Ones)

These usually just work, but let's make them work better.

**Both AMD & Intel:**
```bash
# Basic drivers and Vulkan support
sudo dnf install -y mesa-dri-drivers mesa-vulkan-drivers vulkan-loader mesa-libGLU
```

**AMD:**
```bash
# AMD video acceleration (makes videos smoother)
sudo dnf install -y mesa-va-drivers-freeworld mesa-vdpau-drivers-freeworld
```

**Newer Intel GPUs:**
```bash
# Intel video acceleration (for newer Intel GPUs)
sudo dnf install -y intel-media-driver
```
**Older Intel GPUs:**
```bash
# Intel video acceleration (for Grandfather Intel GPUs)
sudo dnf install libva-intel-driver
```

## 🎵 Making Media Work

### Video Codecs (So Everything Plays)

Fedora ships with basically no codecs because of patent issues. This fixes that.

```bash
# Replace the neutered ffmpeg with the real one
sudo dnf swap -y ffmpeg-free ffmpeg --allowerasing

# Install all the GStreamer plugins
sudo dnf install -y gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav lame\* --exclude=gstreamer1-plugins-bad-free-devel

# Install multimedia groups

sudo dnf4 group install multimedia
sudo dnf group install -y sound-and-video
```

### Hardware Acceleration

This makes video playback use your GPU instead of hammering your CPU.

```bash
# Install VA-API stuff
sudo dnf install -y ffmpeg-libs libva libva-utils

# If you have NVIDIA, add this too
sudo dnf install -y nvidia-vaapi-driver
```

### Firefox Video Fix

Firefox needs a little help with H.264 videos.

```bash
# Install the Cisco codec (it's free but weird licensing)
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264

# Enable the Cisco repo
sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
sudo dnf update -y
```

Restart Firefox and check that the OpenH264 plugin is enabled in `about:addons`.

## 🔧 Useful Stuff

### Archive Support

Because you'll definitely need to extract a .rar file someday.

```bash
sudo dnf install -y p7zip p7zip-plugins unrar
```

### Microsoft Fonts (Unfortunately Still Needed)

Web pages and documents still look weird without these.

```bash
# Install dependencies
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig

# Install the fonts
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm

# Update font cache
sudo fc-cache -fv
```

### AppImage Support

Some apps only come as AppImages. This makes them work.

```bash
# Install FUSE
sudo dnf install -y fuse libfuse2

# Optional: AppImage manager (actually pretty useful)
flatpak install -y flathub it.mijorus.gearlever
```

## ⚡ Making Things Faster

### Faster Boots

This one's easy and makes a noticeable difference.

```bash
sudo systemctl disable NetworkManager-wait-online.service
```

### Better Battery Life

Fedora's power management is pretty good, but if you want more control consider [TLP](https://github.com/linrunner/TLP):

```bash
#First Stop the Power Profiles Daemon
sudo systemctl stop power-profiles-daemon.service

#then Mask it because it make conflict with TLP
sudo systemctl mask power-profiles-daemon.service

# Install TLP (only if you need it)
sudo dnf install -y tlp tlp-rdw

# Enable it
sudo systemctl enable --now tlp

# For analyzing power usage
sudo dnf install -y powertop
```

Only install TLP if Fedora's built-in power management isn't working for you.

### Dual Boot Time Fix

If you dual boot with Windows, this fixes the clock being wrong.

```bash
# Tell Fedora to use UTC for hardware clock
sudo timedatectl set-local-rtc 0 --adjust-system-clock
```

### Performance Tweaks (Proceed with Caution)

**⚠️ WARNING:** This disables important security features. Only do this if you know what you're doing and really need the performance.

```bash
# To disable CPU mitigations (NOT recommended)
# sudo grubby --update-kernel=ALL --args="mitigations=off"

# To re-enable them later (DO THIS)
# sudo grubby --update-kernel=ALL --remove-args="mitigations=off"
```

## 🔒 Security Stuff

### Encrypted DNS (Optional but Cool)

This encrypts your DNS queries so your ISP can't see what websites you're visiting.

```bash
# Install cloudflared
sudo dnf install -y cloudflared

# Create a systemd service for cloudflared
sudo tee /etc/systemd/system/cloudflared.service > /dev/null <<'EOF'
[Unit]
Description=Cloudflared DNS-over-HTTPS proxy
After=network.target

[Service]
ExecStart=/usr/bin/cloudflared proxy-dns --upstream https://1.1.1.1/dns-query --upstream https://1.0.0.1/dns-query
Restart=on-failure
User=nobody
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
EOF

# Reload and enable the service
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now cloudflared

# Configure systemd-resolved to use 127.0.0.1 (cloudflared)
sudo mkdir -p /etc/systemd/resolved.conf.d
sudo tee /etc/systemd/resolved.conf.d/dns-over-https.conf > /dev/null <<'EOF'
[Resolve]
DNS=127.0.0.1
FallbackDNS=1.1.1.1
DNSSEC=yes
Cache=yes
EOF

# Tell NetworkManager to use systemd-resolved
sudo tee /etc/NetworkManager/conf.d/dns.conf > /dev/null <<'EOF'
[main]
dns=systemd-resolved
EOF

# Restart services
sudo systemctl restart cloudflared
sudo systemctl restart systemd-resolved
sudo systemctl restart NetworkManager

# Test DNS resolution
dig +short example.com

# Check current DNS status
resolvectl status
```

## 💾 Backup Your Stuff

### System Snapshots

If you're using Btrfs (default in newer Fedora), you can take snapshots with [btrfs assistant](https://gitlab.com/btrfs-assistant/btrfs-assistant).

```bash
sudo dnf install -y btrfs-assistant btrbk snapper
```

Setup:
1. Run `btrfs-assistant` to set up snapshots with a GUI
2. Enable automatic snapshots:
```bash
sudo systemctl enable --now snapper-timeline.timer
sudo systemctl enable --now snapper-cleanup.timer
```

**Important:** This only backs up system files, not your personal stuff unless you configure it differently.

### Personal Files

Use [Déjà Dup](https://gitlab.gnome.org/World/deja-dup) for your documents, photos, etc.

```bash
# Install it
sudo dnf install -y deja-dup

# Or get it from Flathub
flatpak install -y flathub org.gnome.DejaDup
```

**Always backup to external storage or cloud, not the same disk.**

## 🎮 Gaming Setup

### Steam and Gaming

[Steam](https://store.steampowered.com/) works great on Fedora thanks to Proton.

```bash
sudo dnf install -y steam
```

That's it. Seriously. Most Windows games just work now.

## 🌟 Apps I Actually Use

### Browsers

**[Brave](https://brave.com/)** (blocks ads, pretty fast):
```bash
sudo dnf install dnf-plugins-core
sudo dnf config-manager addrepo --from-repofile=https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo
sudo dnf install brave-browser
```

**[Vivaldi](https://vivaldi.com/)** (tons of customization options):
```bash
sudo dnf config-manager --add-repo https://repo.vivaldi.com/archive/vivaldi-fedora.repo
sudo rpm --import https://repo.vivaldi.com/archive/linux_signing_key.pub
sudo dnf install -y vivaldi-stable
```

### Development

**[VS Code](https://code.visualstudio.com/):**
```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf install -y code
```

**[JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/)** (for Rider, Android Studio, And Jetbrains I use it for Rider IDE)
```bash
TMP_DIR=$(mktemp -d)
ARCHIVE_URL=$(curl -s 'https://data.services.jetbrains.com/products/releases?code=TBA&latest=true&type=release' \
  | jq -r '.TBA[0].downloads.linux.link')
curl -Lo "$TMP_DIR/toolbox.tar.gz" "$ARCHIVE_URL"
mkdir -p "$HOME/.local/share/JetBrains/Toolbox"
tar -xzf "$TMP_DIR/toolbox.tar.gz" --strip-components=1 -C "$HOME/.local/share/JetBrains/Toolbox"
"$HOME/.local/share/JetBrains/Toolbox/jetbrains-toolbox" &
```
THEN REMOVE THE TEMP WECREATED TO INSTALL JetBrains Toolbox WITH:
```bash
rm -rf "$TMP_DIR"
```

**[Git](https://git-scm.com/):**
```bash
sudo dnf install -y git
```

**Containers ([Podman](https://podman.io/) is better than Docker, fight me):**
```bash
sudo dnf install -y podman podman-compose podman-docker
```

### Office Work

**[LibreOffice](https://www.libreoffice.org/)** (probably already installed):
```bash
sudo dnf install -y libreoffice-writer libreoffice-calc libreoffice-impress libreoffice-draw
```

**[OnlyOffice](https://www.onlyoffice.com/)** (better MS Office compatibility):
```bash
sudo dnf install -y https://download.onlyoffice.com/install/desktop/editors/linux/onlyoffice-desktopeditors.x86_64.rpm
```

## 🖥️ Desktop Environment
### GNOME

**Stop GNOME Software autostart** This thing launches every boot and just sits there hogging memory. I always remove it:

```bash
sudo rm /etc/xdg/autostart/org.gnome.Software.desktop
```

**Get GNOME Tweaks** You literally can't use GNOME without this. Need to add minimize/maximize buttons to Window? This is where you do it:

```bash
sudo dnf install gnome-tweaks
```

**Extra themes if you're into that** I stick with default but some people like options. Up to you:

```bash
sudo dnf install gnome-themes-extra
```

**Extension Manager is essential** Don't even think about skipping this one. Makes managing extensions actually easy:

```bash
flatpak install -y flathub com.mattjakeman.ExtensionManager
```

**My must-have extensions:** Its Just my personal preference do what you want :)

- [Vitals](https://extensions.gnome.org/extension/1460/vitals/) - shows CPU/RAM usage in top bar, pretty handy
- [Blur my Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/) - makes everything look less flat and boring
- [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/) - turns the dock into something actually useful


### KDE
nothing for now XD


## 🧹 Keeping Things Clean

### Removing Bloat

**Be careful here.** Only remove stuff you're sure you don't need.

```bash
# See what's installed
dnf list installed | less

# Check what depends on a package before removing it
dnf info package-name
dnf repoquery --whatrequires package-name

# Example removals (modify for your needs)
# sudo dnf remove -y cheese rhythmbox
```

### System Cleanup

Do this occasionally to free up space.

```bash
# Clean package cache
sudo dnf clean all

# Remove orphaned packages
sudo dnf autoremove -y

# Remove old kernels (if you have too many)
# sudo dnf remove $(dnf repoquery --installonly --latest-limit=-2 -q)
```

---

## 👏 Thanks

This guide exists because a bunch of people before me figured this stuff out and shared it. Big thanks to folks like devangshekhawat, paulsorensen, Mohammed Besar, and countless others on forums and Reddit who've helped people get Fedora working properly.

*This isn't official Fedora documentation - it's just what works for me. Your mileage may vary. Always backup important stuff before making big changes, and don't blame me if something breaks (but feel free to ask for help on the Fedora forums).*

---

**Final note:** Linux can be frustrating sometimes, but it's also incredibly rewarding once you get it set up the way you like. Don't give up if something doesn't work the first time - that's just part of the experience!
