# what to do after installing Fedora 42


**default fedora stuff for better experience:**

```bash
# faster downloads
printf "max_parallel_downloads=10\nfastestmirror=true\n" | sudo tee -a /etc/dnf/dnf.conf > /dev/null

# fix sleep
touch /etc/systemd/logind.conf
echo -e "[Login]\nIdleAction=suspend\nIdleActionSec=1h" | sudo tee /etc/systemd/logind.conf > /dev/null

sudo dnf update

sudo fwupdmgr refresh --force
sudo fwupdmgr get-devices # Lists devices with available updates.
sudo fwupdmgr get-updates # Fetches list of available updates.
sudo fwupdmgr update

reboot
```

```bash
# enabling the rpm repositories
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf group upgrade core

# enbale flatpak
sudo dnf install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-modify --enable flathub

# codecs 
sudo dnf4 group install multimedia
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing # Switch to full FFMPEG.
sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin # Installs gstreamer components. Required if you use Gnome Videos and other dependent applications.
sudo dnf group install -y sound-and-video # Installs useful Sound and Video complementary packages.`

# H/W Video Acceleration
sudo dnf install ffmpeg-libs libva libva-utils


# intel stuff
sudo dnf swap libva-intel-media-driver intel-media-driver --allowerasing
sudo dnf install libva-intel-driver
```

**fix stuff to clone dev-env:**

- setup default directories:

```bash
mkdir $HOME/repos
cd $HOME/repos
mkdir assembly  bash  c  js_ts  keyboards  python
cd
```

- add user.js from https://github.com/yokoffing/Betterfox 
- add openh264:
```bash
# fix firefox (also remember to add betterfox and enable OpenH264 in firefox)
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264
sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
```
- setup passwords directory
- install syncthing and setup syncthing:

```bash
sudo dnf install syncthing
sudo systemctl start syncthing@$(whoami)
systemctl --user is-enabled syncthing.service || sudo systemctl enable syncthing@$(whoami)
```

- install git and password manager `sudo dnf5 install git keepassxc`
- setup keepassxc
- setup ssh in git in the browser and in keepassxc

