# How to use?
#### 1. better download as txt and open, it wasn't make as .md style
#### 2. be careful and read every line, it might destroy your system
#### 3. think twice before using someones script from internet

## Have fun it's just another guys arch script

STUFF archlinux step-by-step !important ------------------

sudo pacman -Syu firefox ibus-hangul telegram-desktop obsidian obs-studio qbittorrent steam fish cmake clang ninja flatpak gnome-software thunderbird ttf-fira-code ttf-roboto ttf-ubuntu-font-family noto-fonts-cjk ttf-baekmuk networkmanager pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber pavucontrol bluez bluez-utils libreoffice-still btop jdk17-openjdk ffmpegthumbs

sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
systemctl --user enable pipewire pipewire-pulse wireplumber
systemctl --user start pipewire pipewire-pulse wireplumber
systemctl --user restart pipewire pipewire-pulse wireplumber
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
sudo systemctl restart bluetooth.service

sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si

curl -sS https://starship.rs/install.sh | sh

starship preset jetpack -o ~/.config/starship.toml

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

curl -fsSL https://ollama.com/install.sh | sh

yay -Syu android-studio visual-studio-code-bin google-chrome ttf-nanum gnu-free-fonts noto-fonts ttf-bitstream-vera ttf-croscore ttf-dejavu ttf-droid ttf-ibm-plex ttf-input ttf-input-nerd ttf-liberation ttf-roboto obs-composite-blur


flatpak install -y flathub com.github.johnfactotum.Foliate com.usebruno.Bruno net.ankiweb.Anki io.httpie.Httpie io.dbeaver.DBeaverCommunity io.github.mhogomchungu.media-downloader org.zealdocs.Zeal com.todoist.Todoist com.vysp3r.ProtonPlus ru.linux_gaming.PortProton rocks.koreader.KOReader io.github.troyeguo.koodo-reader com.github.babluboy.bookworm org.kde.arianna com.calibre_ebook.calibre dev.restfox.Restfox io.github.david_swift.Flashcards xyz.safeworlds.hiit io.gitlab.guillermop.Counters org.gnome.Loupe io.github.getnf.embellish dev.bragefuglseth.Keypunch io.github.revisto.drum-machine io.github.nate_xyz.Resonance net.lutris.Lutris


Установка `.desktop` файлов ярлыков со ссылками -----------------------------------

1. Переместить applications в папку приложений по пути:
~/.local/share/applications/

2. Сделать его исполняемым:

chmod +x ~/.local/share/applications/*.desktop

FCITX5 -----------------------------------------------------------------------------------------

sudo pacman -Syu
sudo pacman -S fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-qt fcitx5-hangul

опционально, по языкам:

sudo pacman -S fcitx5-mozc     Japanese
sudo pacman -S fcitx5-rime     Rime (Chinese)
sudo pacman -S fcitx5-hangul   Hangul (Korean)

System Settings → Input Devices → Virtual Keyboard и выберите Fcitx 5 (KWin запустит процесс правильно в Wayland).

code ~/.config/environment.d/fcitx5.conf

Paste inside file

GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
INPUT_METHOD=fcitx
SDL_IM_MODULE=fcitx


mkdir -p ~/.config/autostart

cp /usr/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart/


code ~/.config/fcitx5/config


[Hotkey/TriggerKeys]

0=Super+space
1=Zenkaku_Hankaku
2=Hangul
3=Shift+space
4=Control+space


Context menu for kde-plasma VSCODE --------------------------------------------------------------


mkdir -p ~/.local/share/kio/servicemenus
cat > ~/.local/share/kio/servicemenus/open-with-vscode.desktop <<'EOF'
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

    Перезапусти Dolphin (закрой все окна Dolphin и запусти снова). Если хочешь — можно сделать так:

killall dolphin 2>/dev/null || true
dolphin & disown



Add DNS SERVER except local ---------------------------------------------------------------------------

nmcli connection show 

nmcli con mod "Wired connection 1" ipv4.dns "1.1.1.1,1.0.0.1,192.168.1.1,8.8.8.8,8.8.4.4"
nmcli con mod "Wired connection 1" ipv4.dns-search "localnet"
nmcli con mod "Wired connection 1" ipv4.ignore-auto-dns yes
nmcli con up "Wired connection 1"
cat /etc/resolv.conf


nmcli con mod "Wired connection 1" ipv4.dns "192.168.1.1,1.1.1.1,8.8.8.8"


code /etc/resolv.conf  

below localnet if you have one

nameserver 1.1.1.1
nameserver 8.8.8.8

sudo pacman -Syy for update changes


DOCKER INSTALLATION -------------------------------------------------------------------------


sudo pacman -S docker docker-compose docker-buildx

sudo systemctl enable docker.service
sudo systemctl enable docker.socket
sudo systemctl start docker.socket
sudo systemctl start docker.service

sudo systemctl status docker.service

sudo usermod -aG docker bart

groups bart

newgrp docker

docker run hello-world

sudo reboot

WINE --------------------------------------------

sudo pacman -Syu \
wine wine-mono wine-gecko winetricks \
lib32-mesa lib32-libgl lib32-alsa-plugins lib32-libpulse \
lib32-openal lib32-vulkan-icd-loader



GNOME-TERMINAL ---------------------------------

sudo pacman -S gnome-terminal
тема для gnome-terminal
bash -c "$(wget -qO- https://git.io/vQgMr)"


GNOME -----------------------------------

sudo pacman -S networkmanager
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

Clipboard Indicator: Добавляет индикатор буфера обмена в верхнюю панель.
sudo pacman -S gnome-shell-extension-clipboard-indicator


KDE ---------------------------------------

sudo pacman -S gtk2 gtk3 gtk4
sudo pacman -S gtk-engine-murrine
sudo pacman -S breeze-gtk breeze-icons
sudo pacman -S breeze-icons noto-fonts


KVM ANDROID_EMULATOR SUPPORT --------------------------------------------

sudo pacman -S virt-manager qemu vde2 ebtables iptables-nft nftables dnsmasq bridge-utils ovmf


code /etc/libvirt/libvirtd.conf

unix_sock_group = "libvirt"

unix_sock_rw_perms = "0770"

sudo usermod -a -G kvm,libvirt $(whoami)

sudo groups $(whoami)

sudo systemctl enable libvirtd
sudo systemctl start libvirtd

code /etc/libvirt/qemu.conf
Search for this lines and uncomment them

user = "root"

group = "root"
You will need to replace root with your username, so they look like this

user = "myuser"

group = "myuser"

sudo systemctl restart libvirtd

sudo virsh net-autostart default
