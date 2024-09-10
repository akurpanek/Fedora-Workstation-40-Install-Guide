# Fedora 40 Workstation Setup

Auhor: André Kurpanek
Erstellt am: DD.MM.YYYY; Aktualisiert am: DD.MM.YYYY

---

## OS Installation

### Tastatur

### Partitionierung

---

## Post OS-Installation

### Setup RPM Fusion

```shell
# Installing Free and Nonfree Repositories
sudo dnf install -y \
    https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
    https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

```shell
# Activating OpenH264 Repository
sudo dnf config-manager --enable -y fedora-cisco-openh264
```

```shell
# Switch to full ffmpeg
sudo dnf swap -y ffmpeg-free ffmpeg --allowerasing
```

```shell
# Install additional codec
sudo dnf update -y @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf update -y @sound-and-video
```

```shell
# Install Intel Media Driver for VAAPI (recent)
sudo dnf install -y intel-media-driver
```

```shell
# Install DVD Support
sudo dnf install -y rpmfusion-free-release-tainted
sudo dnf install -y libdvdcss
```

```shell
# Update AppStream metadata
sudo dnf update -y @core
```

### Intel Gen12 Fixes

References:
- <https://haominnn.medium.com/fixing-fedora-38-screen-flicking-issue-on-intel-integrated-graphics-620-thinkpad-x270-74020871dfb0>


```shell
# Fix Screen Flickering Issue with Intel Integrated Graphics
grep -iq '^GRUB_CMDLINE_LINUX=.*i915.enable_psr' /etc/default/grub \
  || sudo sed -i 's#^\(GRUB_CMDLINE_LINUX=".*\)"$#\1 i915.enable_psr=0"#' /etc/default/grub
```

```shell
# Fix Screen Flickering Issue with Intel Integrated Graphics
grep -iq '^GRUB_CMDLINE_LINUX=.*i915.enable_guc' /etc/default/grub \
  || sudo sed -i 's#^\(GRUB_CMDLINE_LINUX=".*\)"$#\1 i915.enable_guc=3"#' /etc/default/grub
```

```shell
# Intel® oneAPI Toolkits Installation Guide for Linux* OS
# Validate
sudo dnf install -y glx-utils glmark2
DRI_PRIME=0 glxinfo -B | grep -i device
DRI_PRIME=1 glxinfo -B | grep -i device

sudo yum install intel-basekit

```
-<https://www.intel.com/content/www/us/en/docs/oneapi/installation-guide-linux/2023-0/yum-dnf-zypper.html>


```shell
# Apply tinny and quiet sound fix for bass speakers and internal microphone
grep -iq '^options snd-sof-intel-hda-generic hda_model=alc287-yoga9-bass-spk-pin' \
  /etc/modprobe.d/snd.conf \
  || echo "options snd-sof-intel-hda-generic hda_model=alc287-yoga9-bass-spk-pin" \
  | sudo tee -a /etc/modprobe.d/snd.conf
```

### Setup Automatic Updates

```shell
# Install dnf-automatic
sudo dnf install -y dnf-automatic
systemctl enable --now dnf-automatic.timer
```

```shell
# Settings of dnf-automatic
```

### Power Management

References:
- <https://fedoraproject.org/wiki/Changes/TunedAsTheDefaultPowerProfileManagementDaemon>

```shell
# Switch to Tuned Power Profile Management Daemon
sudo dnf swap -y power-profiles-daemon tuned-ppd
```

sudo dnf install -y powertop






