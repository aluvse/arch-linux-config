

# Arch Linux Setup Guide

Complete workstation configuration for development and productivity.

## System Configuration

### Package Management
```bash
# Update system and install essential packages
sudo pacman -Syu firefox ibus-hangul telegram-desktop obsidian qbittorrent steam fish cmake clang ninja flatpak gnome-software thunderbird ttf-fira-code ttf-roboto ttf-ubuntu-font-family noto-fonts-cjk ttf-baekmuk networkmanager pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber pavucontrol bluez bluez-utils libreoffice-still btop jdk17-openjdk ffmpegthumbs
```

### Service Management
```bash
# Enable and start essential services
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

# User services for audio
systemctl --user enable pipewire pipewire-pulse wireplumber
systemctl --user start pipewire pipewire-pulse wireplumber
systemctl --user restart pipewire pipewire-pulse wireplumber

# Bluetooth services
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
sudo systemctl restart bluetooth.service
```

## Development Environment

### AUR Helper Installation
```bash
# Install yay AUR helper
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

### Development Tools
```bash
# Install starship prompt
curl -sS https://starship.rs/install.sh | sh
starship preset jetpack -o ~/.config/starship.toml

# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Install additional development tools and fonts
yay -Syu android-studio visual-studio-code-bin google-chrome ttf-nanum gnu-free-fonts noto-fonts ttf-bitstream-vera ttf-croscore ttf-dejavu ttf-droid ttf-ibm-plex ttf-input ttf-input-nerd ttf-liberation ttf-roboto gobject-introspection python-setuptools python-wheel meson
```

### JetBrains IDEs (Optional)
```bash
# Install JetBrains development tools
yay -Syu goland goland-jre webstorm webstorm-jre clion clion-jre clion-cmake phpstorm phpstorm-jre rustrover rustrover-jre rider pycharm-professional intellij-idea-ultimate-edition datagrip datagrip-jre

# Exclude JetBrains packages from system updates
echo "IgnorePkg = goland goland-jre webstorm webstorm-jre clion clion-jre clion-cmake phpstorm phpstorm-jre rustrover rustrover-jre rider pycharm-professional intellij-idea-ultimate-edition datagrip datagrip-jre" | sudo tee -a /etc/pacman.conf
```

## Flatpak Applications
```bash
# Install various Flatpak applications
flatpak install -y flathub com.github.johnfactotum.Foliate com.usebruno.Bruno net.ankiweb.Anki io.httpie.Httpie io.dbeaver.DBeaverCommunity io.github.mhogomchungu.media-downloader org.zealdocs.Zeal com.todoist.Todoist com.vysp3r.ProtonPlus ru.linux_gaming.PortProton rocks.koreader.KOReader io.github.troyeguo.koodo-reader com.github.babluboy.bookworm org.kde.arianna com.calibre_ebook.calibre dev.restfox.Restfox io.github.david_swift.Flashcards xyz.safeworlds.hiit io.gitlab.guillermop.Counters org.gnome.Loupe io.github.getnf.embellish dev.bragefuglseth.Keypunch io.github.revisto.drum-machine io.github.nate_xyz.Resonance net.lutris.Lutris flathub org.videolan.VLC com.protonvpn.www
```

## Desktop Shortcuts
```bash
# Install .desktop files to application directory
mkdir -p ~/.local/share/applications/
# Move your .desktop files to ~/.local/share/applications/

# Make desktop files executable
chmod +x ~/.local/share/applications/*.desktop
```

## Input Method Configuration (FCITX5)

### Installation
```bash
sudo pacman -Syu
sudo pacman -S fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-qt fcitx5-hangul

# Optional language support
sudo pacman -S fcitx5-mozc     # Japanese
sudo pacman -S fcitx5-rime     # Rime (Chinese)
sudo pacman -S fcitx5-hangul   # Hangul (Korean)
```

### Environment Configuration
```bash
# Create environment configuration
mkdir -p ~/.config/environment.d
cat > ~/.config/environment.d/fcitx5.conf << 'EOF'
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
INPUT_METHOD=fcitx
SDL_IM_MODULE=fcitx
EOF
```

### Autostart Configuration
```bash
mkdir -p ~/.config/autostart
cp /usr/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart/
```

### Hotkey Configuration
```bash
# Configure Fcitx5 hotkeys
mkdir -p ~/.config/fcitx5
cat > ~/.config/fcitx5/config << 'EOF'
[Hotkey/TriggerKeys]
0=Super+space
1=Zenkaku_Hankaku
2=Hangul
3=Shift+space
4=Control+space
EOF
```

## KDE Plasma Context Menu

### VS Code Context Menu
```bash
mkdir -p ~/.local/share/kio/servicemenus
cat > ~/.local/share/kio/servicemenus/open-with-vscode.desktop << 'EOF'
[Desktop Entry]
Type=Service
MimeType=inode/directory;application/octet-stream;text/plain;
Actions=openWithVSCode
X-KDE-Priority=TopLevel
Name=Open with Visual Studio Code
Icon=visual-studio-code

[Desktop Action openWithVSCode]
Name=Open with Visual Studio Code
Icon=visual-studio-code
Exec=/usr/bin/code %F
EOF

chmod +x ~/.local/share/kio/servicemenus/open-with-vscode.desktop

# Restart Dolphin
killall dolphin 2>/dev/null || true
dolphin & disown
```

## Network Configuration

### DNS Setup
```bash
# Configure DNS servers
nmcli con mod "Wired connection 1" ipv4.dns "9.9.9.9 1.1.1.1 223.5.5.5"
nmcli con mod "Wired connection 1" ipv6.dns "2620:fe::fe 2606:4700:4700::1111 2400:3200::1"
nmcli con mod "Wired connection 1" ipv6.method "auto"

# Apply network changes
nmcli con down "Wired connection 1" && nmcli con up "Wired connection 1"
```

## Docker Installation

### Docker Setup
```bash
sudo pacman -S docker docker-compose docker-buildx

sudo systemctl enable docker.service
sudo systemctl enable docker.socket
sudo systemctl start docker.socket
sudo systemctl start docker.service

# Verify Docker status
sudo systemctl status docker.service

# Add user to docker group
sudo usermod -aG docker $USER

# Verify group membership
groups $USER

# Apply group changes
newgrp docker

# Test Docker installation
docker run hello-world

sudo reboot
```

## Wine Configuration

### Wine Installation
```bash
sudo pacman -Syu wine wine-mono wine-gecko winetricks lib32-mesa lib32-libgl lib32-alsa-plugins lib32-libpulse lib32-openal lib32-vulkan-icd-loader
```

## Terminal Configuration

### GNOME Terminal
```bash
sudo pacman -S gnome-terminal
# Install gnome-terminal theme
bash -c "$(wget -qO- https://git.io/vQgMr)"
```

## Desktop Environment Setup

### GNOME Extensions
```bash
# Clipboard indicator extension
sudo pacman -S gnome-shell-extension-clipboard-indicator
```

### KDE Plasma
```bash
sudo pacman -S gtk2 gtk3 gtk4 gtk-engine-murrine breeze-gtk breeze-icons breeze-icons noto-fonts
```

## Virtualization (KVM)

### KVM Setup for Android Emulator
```bash
sudo pacman -S virt-manager qemu vde2 ebtables iptables-nft nftables dnsmasq bridge-utils ovmf

# Configure libvirt
sudo sed -i 's/#unix_sock_group = "libvirt"/unix_sock_group = "libvirt"/' /etc/libvirt/libvirtd.conf
sudo sed -i 's/#unix_sock_rw_perms = "0770"/unix_sock_rw_perms = "0770"/' /etc/libvirt/libvirtd.conf

# Add user to virtualization groups
sudo usermod -a -G kvm,libvirt $(whoami)

# Verify group membership
sudo groups $(whoami)

# Enable and start libvirt
sudo systemctl enable libvirtd
sudo systemctl start libvirtd

# Configure QEMU user settings
sudo sed -i 's/^#*user = ".*"/user = "$(whoami)"/' /etc/libvirt/qemu.conf
sudo sed -i 's/^#*group = ".*"/group = "$(whoami)"/' /etc/libvirt/qemu.conf

sudo systemctl restart libvirtd

# Enable default network
sudo virsh net-autostart default
```



License

MIT License - feel free to use and modify.
