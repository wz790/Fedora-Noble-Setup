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

- [First Things](#-first-things)
  - [RPM Fusion](#rpm-fusion)
  - [Updates](#updates)
  - [Firmware Updates](#firmware-updates)
  - [Give Your Computer a Name](#give-your-computer-a-name)
- [Getting More Software](#-getting-more-software)
  - [Flathub Setup](#flathub-setup)
- [Graphics Drivers](#-graphics-drivers)
  - [NVIDIA](#nvidia)
  - [AMD & Intel](#amd--intel)
- [Making Media Work](#-making-media-work)
  - [Video Codecs](#video-codecs-so-everything-plays)
  - [Hardware Acceleration](#hardware-acceleration)
  - [Firefox Video Fix](#firefox-video-fix)
- [Useful Stuff](#-useful-stuff)
  - [Archive Support](#archive-support)
  - [AppImage Support](#appimage-support)
  - [Dual Boot Time Fix](#dual-boot-time-fix)
- [Backup Your Stuff](#-backup-your-stuff)
  - [System Snapshots](#system-snapshots)
  - [Personal Files](#personal-files)
- [Gaming Setup](#-gaming-setup)
  - [Steam and Gaming](#steam-and-gaming)
  - [Lutris](#lutris)
  - [MangoHud](#mangohud)
- [Apps I Actually Use](#-apps-i-actually-use)
  - [Browsers](#browsers)
  - [Development](#development)
  - [Containers](#containers)
  - [Multimedia](#multimedia)
  - [Office Work](#office-work)
  - [System Tools](#system-tools)
- [Desktop Environment](#️-desktop-environment)
  - [GNOME](#gnome)
  - [KDE Plasma](#kde-plasma)
- [Keeping Things Clean](#-keeping-things-clean)
  - [System Cleanup](#system-cleanup)
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

# Update everything so it all plays nice together
sudo dnf group upgrade core -y
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

NVIDIA drivers on Fedora 43 are best installed via RPM Fusion's akmod-nvidia packages, here there are also two options here: enable Secure Boot (preferred for security) or disable it. The choice is yours. 
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

#### Option 1: Secure Boot Enabled (Recommended)
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

## Apps I Use

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

## Desktop Environment

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

## Keeping Things Clean

### System Cleanup

Clean package cache and remove orphaned packages

```bash
sudo dnf clean all

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
