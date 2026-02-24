# ✨ My Fedora Linux Noble Setup Guide (Post-Installation)

<p align="center">
  <img src="src/assets/logo.png" alt="Fedora Setup Logo - Nord Style" width="250"/>
</p>

<div align="center">

[![Fedora](https://img.shields.io/badge/Fedora-42+-blue.svg?style=for-the-badge&logo=fedora)](https://getfedora.org/)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-brightgreen.svg?style=for-the-badge)](https://github.com/yourusername/fedora-setup/graphs/commit-activity)

</div>

---

## 👋 Hey there!

Since you're here, you've probably just installed Fedora and now you're staring at a new desktop wondering, "What next?" I've been there. Fedora requires some pushing after a fresh install :) That's why I put together this guide, which is basically everything I gathered from multiple sources when I switched to Fedora.

This is not an official guide. It's just me sharing what I used, and some settings or personal preferences that you can feel free to skip. Just read everything before copy past.

### 💡 Quick heads up about commands:

- When you see `sudo` at the start, that means "run as admin" it'll ask for your password
- The `-y` flag just means "yes to everything" so you don't have to keep pressing enter
- Copy-paste is your friend, but **always read what you're about to run first**

---

## 📋 Table of Contents

<details>
<summary>Click to expand full guide</summary>

- [🔥 First Things First](#-first-things-first)
  - [RPM Fusion - The Good Stuff](#rpm-fusion---the-good-stuff)
  - [Updates (Boring but Important)](#updates-boring-but-important)
  - [Firmware Updates](#firmware-updates)
  - [Give Your Computer a Name](#give-your-computer-a-name)
- [📦 Getting More Software](#-getting-more-software)
  - [Flathub Setup](#flathub-setup)
- [🎮 Graphics Drivers](#-graphics-drivers)
  - [NVIDIA (The Tricky One)](#nvidia-the-tricky-one)
  - [AMD & Intel (The Easy Ones)](#amd--intel-the-easy-ones)
- [🎵 Making Media Work](#-making-media-work)
  - [Video Codecs (So Everything Plays)](#video-codecs-so-everything-plays)
  - [Hardware Acceleration](#hardware-acceleration)
  - [Firefox Video Fix](#firefox-video-fix)
- [🔧 Useful Stuff](#-useful-stuff)
  - [Archive Support](#archive-support)
  - [Microsoft Fonts (Unfortunately Still Needed)](#microsoft-fonts-unfortunately-still-needed)
  - [AppImage Support](#appimage-support)
  - [Flatpak Auto-Updates](#flatpak-auto-updates)
- [⚡ Making Things Faster](#-making-things-faster)
  - [Faster Boots](#faster-boots)
  - [Better Battery Life](#better-battery-life)
  - [Dual Boot Time Fix](#dual-boot-time-fix)
- [🔒 Security Stuff](#-security-stuff)
  - [Encrypted DNS (Optional but Cool)](#encrypted-dns-optional-but-cool)
- [💾 Backup Your Stuff](#-backup-your-stuff)
  - [System Snapshots](#system-snapshots)
  - [Personal Files](#personal-files)
- [🎮 Gaming Setup](#-gaming-setup)
  - [Steam and Gaming](#steam-and-gaming)
  - [Lutris](#lutris)
  - [MangoHud](#mangohud)
- [🌟 Apps I Actually Use](#-apps-i-actually-use)
  - [Browsers](#browsers)
  - [Development](#development)
  - [Containers](#containers)
  - [Multimedia](#multimedia)
  - [Office Work](#office-work)
  - [System Tools](#system-tools)
- [🖥️ Desktop Environment](#️-desktop-environment)
  - [GNOME](#gnome)
  - [KDE Plasma](#kde-plasma)
- [🧹 Keeping Things Clean](#-keeping-things-clean)
  - [System Cleanup](#system-cleanup)
</details>

---

## 🔥 First Things First

### RPM Fusion - The Good Stuff

Okay, first thing Fedora ships pretty bare-bones because of legal reasons. [RPM](https://rpmfusion.org/) Fusion is where all the actually useful stuff lives (codecs, drivers, etc.). You want this.

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

<details>
<summary>🤔 What's the difference between free and nonfree?</summary>

- **Free**: Open source software that's legally redistributable
- **Nonfree**: Proprietary software (NVIDIA drivers, some codecs) that can't be shipped with Fedora due to licensing

</details>

---

### Updates (Boring but Important)

Yeah, I know, updates are boring. But seriously, do this first. Fresh installs always have outdated packages.

```bash
# Update everything
sudo dnf update -y
```
```bash
# If it updated the kernel, reboot
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

> ⚠️ **Note**: After firmware updates you need a reboot. Just do it.

---

### Give Your Computer a Name

This is purely cosmetic but makes you feel more at home. Pick something fun!

```bash
# Replace with whatever you want
sudo hostnamectl set-hostname hungry-beast
```

---

## 📦 Getting More Software

### Flathub Setup

Fedora comes with a neutered version of [Flatpak](https://flatpak.org/). [Flathub](https://flathub.org/) is where the actual apps are.

```bash
# Remove the limited Fedora repo
flatpak remote-delete fedora
```
Here i will let you decide there are a few options. Choose the one that suits you best. Personally, I would choose the first option, but you can decide for yourself here some info from [flathub](https://docs.flathub.org/docs/for-users/installation#remove-subsets).

#### Option 1: Full Repository:

Get access to everything Flathub (that include apps that are not officially maintained by their developers):

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Option 2: Verified Apps Only:

If you want only apps maintained by their actual developers go with this:

```bash
flatpak remote-add --if-not-exists --subset=verified flathub https://flathub.org/repo/flathub.flatpakrepo
```

**Note:** Some popular apps won't show up because they're packaged by the community.

#### Option 3: Open Source Only:

This for only free and open source software:

```bash
flatpak remote-add --if-not-exists --subset=floss flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Option 4: Verified + Open Source:

The most restrictive option it include only apps that are both verified by developers and open source:

```bash
flatpak remote-add --if-not-exists --subset=verified_floss flathub https://flathub.org/repo/flathub.flatpakrepo
```
---

## 🎮 Graphics Drivers

### NVIDIA (The Tricky One)

> ⚠️ **WARNING**: NVIDIA on Linux is complicated for now this works most of the time, but if you have issues, welcome to the club.

#### 📋 Before you start:

- Turn off **Secure Boot** in your BIOS (or learn to sign kernel modules your choice ;). )
- Make sure everything is **updated and rebooted**
- Have a backup plan (seriously it's not my responsibility.)

```bash
# Install kernel headers and dev tools
sudo dnf install -y kernel-devel kernel-headers gcc make dkms acpid \
  libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig

# Enable RPM Fusion (if not already done)
sudo dnf install -y \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

> 📌 **Special Note for RTX 4000 and Newer Series:**
> 
> If you're using a 4000 series or newer GPU, Fedora needs a build macro set before installing the driver. This enables the open kernel module.

```bash
# Set open kernel module macro (one-time step for RTX 4000+)
sudo sh -c 'echo "%_with_kmod_nvidia_open 1" > /etc/rpm/macros.nvidia-kmod'
```

#### Install the NVIDIA driver:

```bash
sudo dnf install -y akmod-nvidia xorg-x11-drv-nvidia-cuda
```

Now **wait**. Seriously. It takes 5–15 minutes to build the module.

You can monitor progress with:

```bash
sudo journalctl -f -u akmods
```

Once done:

```bash
sudo reboot
```

#### ✅ Check if it worked:

```bash
nvidia-smi
```

You should see your GPU info. If not, try this:

```bash
# Force rebuild and try again
sudo akmods --force --kernels $(uname -r)
sudo reboot
```

<details>
<summary>🆘 Help! I'm stuck at 800×600 / black screen / terminal!</summary>

The NVIDIA module probably didn't build correctly. Here's what to do:

1. **Boot into an older kernel**:
   - At GRUB menu, select "Advanced Options"
   - Pick the previous kernel version
   
2. **Force rebuild**:
   ```bash
   sudo akmods --kernels $(uname -r) --rebuild
   sudo reboot
   ```

3. **Still broken?** Check the logs:
   ```bash
   sudo journalctl -u akmods
   dmesg | grep -i nvidia
   ```

</details>

> 💡 **Tip**: After kernel updates, especially on newer GPUs, you might need to manually rebuild:
> 
> ```bash
> sudo akmods --kernels $(uname -r) --rebuild
> sudo reboot
> ```

---

### AMD & Intel (The Easy Ones)

These usually just work, but let's make them work **better**.

#### Both AMD & Intel:

```bash
# Basic drivers and Vulkan support
sudo dnf install -y mesa-dri-drivers mesa-vulkan-drivers vulkan-loader mesa-libGLU
```

#### AMD Only:

```bash
# AMD video acceleration (makes videos smoother)
sudo dnf install -y mesa-va-drivers-freeworld mesa-vdpau-drivers-freeworld
```

#### Intel (Newer GPUs):

```bash
# Intel video acceleration (for newer Intel GPUs)
sudo dnf install -y intel-media-driver
```

#### Intel (Older GPUs):

```bash
# Intel video acceleration (for Grandfather Intel GPUs)
sudo dnf install -y libva-intel-driver
```

> ✅ **That's it!** AMD and Intel are usually plug-and-play.

---

## 🎵 Making Media Work

### Video Codecs (So Everything Plays)

Fedora ships with basically no codecs because of patent issues. This fixes that.

```bash
# Replace the neutered ffmpeg with the real one
sudo dnf swap -y ffmpeg-free ffmpeg --allowerasing

# Install all the GStreamer plugins
sudo dnf install -y gstreamer1-plugins-{bad-\*,good-\*,base} \
  gstreamer1-plugin-openh264 gstreamer1-libav lame\* \
  --exclude=gstreamer1-plugins-bad-free-devel

# Install multimedia groups
sudo dnf group install -y multimedia
sudo dnf group install -y sound-and-video
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

```bash
# Install the Cisco codec (it's free but weird licensing)
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264

# Enable the Cisco repo
sudo dnf config-manager --set-enabled fedora-cisco-openh264
sudo dnf update -y
```

> ⚠️ **Important**: Restart Firefox and check that the OpenH264 plugin is enabled in `about:addons`.

---

## 🔧 Useful Stuff

### Archive Support

Because you'll definitely need to extract a `.rar` file someday.

```bash
sudo dnf install -y p7zip p7zip-plugins unrar
```

---

### Microsoft Fonts (Unfortunately Still Needed)

Web pages and documents still look weird without these.

```bash
# Install dependencies
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig

# Install the fonts
sudo rpm -i --nodigest --nosignature https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm

# Update font cache
sudo fc-cache -fv
```

> Yes, we still need Arial and Times New Roman. Blame Microsoft ;).

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

### Flatpak Auto-Updates

You can keep your Flatpak apps up to date automatically. This setup updates your Flatpaks every 24 hours and is especially helpful if you disable GNOME Software on startup.

```bash
# Create the service unit
sudo tee /etc/systemd/system/flatpak-update.service > /dev/null <<'EOF'
[Unit]
Description=Update Flatpak apps automatically

[Service]
Type=oneshot
ExecStart=/usr/bin/flatpak update -y --noninteractive
EOF

# Create the timer unit
sudo tee /etc/systemd/system/flatpak-update.timer > /dev/null <<'EOF'
[Unit]
Description=Run Flatpak update every 24 hours
Wants=network-online.target
Requires=network-online.target
After=network-online.target

[Timer]
OnBootSec=120
OnUnitActiveSec=24h

[Install]
WantedBy=timers.target
EOF

# Reload systemd and enable the timer
sudo systemctl daemon-reload
sudo systemctl enable --now flatpak-update.timer

# Check the status to verify everything is working
sudo systemctl status flatpak-update.timer
```

> 🎯 **What this does**: Updates Flatpaks 2 minutes after boot, then every 24 hours. Set it and forget it!

---

## ⚡ Making Things Faster

### Faster Boots

This one's easy and makes a noticeable difference.

```bash
sudo systemctl disable NetworkManager-wait-online.service
```

> 💡 **Why?** This service waits for network connections during boot. Most people don't need this delay.

---

### Better Battery Life

> 🚨 **CAUTION**: Don't mess with this unless you know what you're doing, my friendo ;)

Fedora's power management is pretty good, but I personally prefer [TLP](https://linrunner.de/tlp/) for my laptop:

**Official TLP docs**: [TLP Documentation](https://linrunner.de/tlp/introduction.html)
```bash
# Add TLP Repository 
sudo dnf install -y \
  https://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release.fc$(rpm -E %fedora).noarch.rpm

# Remove conflicting power profiles daemon
sudo dnf remove -y tuned tuned-ppd

# Install TLP
sudo dnf install -y tlp tlp-rdw

# Enable TLP service 
sudo systemctl enable tlp.service

# Mask the following services to avoid conflicts
sudo systemctl mask systemd-rfkill.service systemd-rfkill.socket
```

> ⚠️ **Important**: Only install TLP if Fedora's built-in power management isn't working for you.

---

### Dual Boot Time Fix

If you dual boot with Windows, this fixes the clock being wrong.

```bash
# Tell Fedora to use UTC for hardware clock
sudo timedatectl set-local-rtc 0 --adjust-system-clock
```

> 🤓 **Explanation**: Windows uses local time for hardware clock, Linux uses UTC. This tells Linux to play nice.

---

## 🔒 Security Stuff

### Encrypted DNS (Optional but Cool)

This encrypts your DNS queries so your ISP can't see what websites you're visiting.

> ⚠️ **WARNING**:This global DoT setup with resolve-mode=exclusive breaks captive portal detection on voucher-based/public WiFi networks (hotels, apartments, cafes, hotspots).

<summary>## 🆘 Emergency Fix: Disable Global DoT (For Public WiFi/Hotels)

**If you followed the Encrypted DNS setup and now can't access captive portal login pages** (hotels, apartments, cafes, airports) this will restore normal internet instantly.

### ✅ Quick Disable (2 seconds):
```bash
sudo systemctl stop dnsconfd && sudo rm /etc/NetworkManager/conf.d/global-dot.conf && sudo systemctl restart NetworkManager
```

**What this does**:
- `stop dnsconfd` → Stops encrypted DNS service
- `rm global-dot.conf` → Removes the "force DoT everywhere" rule  
- `restart NetworkManager` → Reverts to normal DHCP DNS

### 🔄 Re enable later (Home network only for now):
```bash
sudo systemctl enable --now dnsconfd
sudo tee /etc/NetworkManager/conf.d/global-dot.conf > /dev/null <<EOF
[main]
dns=dnsconfd
[global-dns]
resolve-mode=exclusive
[global-dns-domain-*]
servers=dns+tls://1.1.1.1#one.one.one.one
EOF
sudo systemctl restart NetworkManager
```

**Install the packages:**
```bash
sudo dnf install dnsconfd
```

**Stop and disable systemd-resolved to avoid conflicts:**
```bash
sudo systemctl disable --now systemd-resolved
sudo systemctl mask systemd-resolved
sudo systemctl enable --now dnsconfd
```

**Configure NetworkManager to use Cloudflare's DNS:**
```bash
sudo mkdir -p /etc/NetworkManager/conf.d
sudo tee /etc/NetworkManager/conf.d/global-dot.conf > /dev/null <<EOF
[main]
dns=dnsconfd

[global-dns]
resolve-mode=exclusive

[global-dns-domain-*]
servers=dns+tls://1.1.1.1#one.one.one.one
EOF

sudo systemctl restart NetworkManager
```

<details>
<summary>🔍 How to verify it's working</summary>


Test that DNS actually works:

got to https://one.one.one.one/help/ and check the results.

You should see **"Using DNS over TLS (DoT) - Yes"** 

If it say Yes, you're all set!
</details>

---

## 💾 Backup Your Stuff

### System Snapshots

If you're using Btrfs (default in newer Fedora), you can take snapshots with btrfs assistant.

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

From fedora repos
```bash
# Install it
sudo dnf install -y deja-dup
```

Or get it from Flathub
```bash
flatpak install -y flathub org.gnome.DejaDup
```

> 💡 **Tip**: Always backup to external storage or cloud, not the same disk.

---

## 🎮 Gaming Setup

### Steam and Gaming

[Steam](https://store.steampowered.com/) works great on Fedora thanks to Proton.

```bash
sudo dnf install -y steam
```

That's it. Seriously. Most Windows games just work now.

> 📌 **Note**: If you're using a newer NVIDIA GPU and Steam doesn't launch or acts weird, it's probably a missing dependency issue. Remove the RPM version and install the Flatpak instead:

```bash
# Remove Steam RPM version 
sudo dnf remove steam

# Install Steam Flatpak version 
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

## 🌟 Apps I Actually Use

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

[VSCodium](https://vscodium.com) is a community-driven, freely-licensed binary distribution of Microsoft’s editor VS Code.


```bash
# Add the GPG key of the repository:
sudo rpmkeys --import https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg

# Add VSCodium repository
printf "[gitlab.com_paulcarroty_vscodium_repo]\nname=download.vscodium.com\nbaseurl=https://download.vscodium.com/rpms/\nenabled=1\ngpgcheck=1\nrepo_gpgcheck=1\ngpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg\nmetadata_expire=1h\n" | sudo tee -a /etc/yum.repos.d/vscodium.repo

# Install VS Codium
sudo dnf install codium
```

---

### Containers

Podman is better than Docker, fight me :)

```bash
# Podman core
sudo dnf install -y podman podman-compose podman-docker

# Podman GUI
flatpak install -y flathub io.podman_desktop.PodmanDesktop
```

---

### Multimedia

[VLC](https://www.videolan.org/vlc/) is my go-to multimedia player, but it's totally up to you whether you use it or choose something else:

```bash
# Install VLC
sudo dnf install -y vlc
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

### Office Work

[OnlyOffice](https://www.onlyoffice.com/) for better for MS Office compatibility:

```bash
flatpak install -y flathub org.onlyoffice.desktopeditors
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

## 🖥️ Desktop Environment

### GNOME

#### Stop GNOME Software autostart

This thing launches every boot and just sits there hogging memory. I always remove it:

```bash
sudo rm /etc/xdg/autostart/org.gnome.Software.desktop
```

> ⚠️ **Note**: Removing GNOME Software from autostart disables update notifications. You'll need to manually check for updates or use the auto-update methods mentioned above.

---

#### Get GNOME Tweaks

You literally can't use GNOME without this. Need to add minimize/maximize buttons? This is where you do it:

```bash
sudo dnf install -y gnome-tweaks
```

---

#### Extra themes (if you're into that)

I stick with default but some people like options. Up to you:

```bash
sudo dnf install -y gnome-themes-extra
```

---

#### Extension Manager is essential

Don't even think about skipping this one. Makes managing extensions actually easy:

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

Do this occasionally to free up space.

```bash
# Clean package cache
sudo dnf clean all

# Remove orphaned packages
sudo dnf autoremove -y
```

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
