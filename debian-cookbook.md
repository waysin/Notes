# Debian Cookbook

Waysin's Debian install process.

## dwm

Install xorg:

`# apt install xserver-xorg-core xservser-xorg-video-intel xserver-xorg-input-libinput xinit`

And config touchpad:

`# vi /etc/X11/xorg.conf.d/30-touchpad.conf`

```conf
Section "InputClass"
    Identifier "libinput touchpad catchall"
    MatchIsTouchpad "on"
    MatchDevicePath "/dev/input/event*"
    Driver "libinput"
    Option "Tapping" "on"
    Option "ClickMethod" "clickfinger"
    Option "NaturalScrolling" "true"
EndSection
```

Install dwm build dependencies:

`# apt install dpkg-dev x11-xserver-utils libx11-dev libxft-dev libxinerama-dev libxrandr-dev`

Download dwm, st, dmenu and slock:

`# apt install curl`

`$ curl -O https://dl.suckless.org/dwm/dwm-6.2.tar.gz -O https://dl.suckless.org/st/st-0.8.4.tar.gz -O https://dl.suckless.org/tools/dmenu-5.0.tar.gz -O https://dl.suckless.org/tools/slock-1.4.tar.gz`

Extract files form archive:

`$ tar -xzvf dwm-6.2.tar.gz`

Into the folder and install:

`$ cd dwm-6.2`

`$ make`

`$ rm config.def.h`

`# make install`

Next st, dmenu and slock performs the same operation...

Set the volume keys for dwm:

`# apt install alsa-utils`

`$ cd dwm-6.2`

`$ vi config.h`

```c
static const char *voldown[] = { "amixer", "-qM", "set", "Master", "5%-", NULL };
static const char *volmute[] = { "amixer", "-q", "set", "Master", "toggle", NULL };
static const char *volup[] = { "amixer", "-qM", "set", "Master", "5%+", NULL };
static Key keys[] = {
    /* ... */
    { 0, 0x1008ff11, spawn, {.v = voldown } },
    { 0, 0x1008ff12, spawn, {.v = volmute } },
    { 0, 0x1008ff13, spawn, {.v = volup } },
    /* ... */
};
```

Setting xinit:

`# apt install acpi`

`$ vi ~/.xinitrc`

```sh
while true
do
    xsetroot -name "$( date +"%A, %b %d %I:%M%p" ) $( acpi -b | awk '{ print $4 } | tr -d ',')"
    sleep 1m
done &
exec dwm
```

Now, run dwm:

`$ startx`

Recommend patch:

- dwm
  - [autostart](https://dwm.suckless.org/patches/autostart/)
- st
  - [anysize](https://st.suckless.org/patches/anysize/)
  - [dracula](https://st.suckless.org/patches/dracula/)
  - [font2](https://st.suckless.org/patches/font2/)
  - [scrollback](https://st.suckless.org/patches/scrollback/)
- dmenu
  - [dmenu_run with command history](https://tools.suckless.org/dmenu/scripts/dmenu_run_with_command_history/)

## zsh

Install zsh and plugins:

`# apt install zsh zsh-autosuggestions zsh-syntaxhighlighting`

Use zsh for default shell:

`$ chsh -s /usr/bin/zsh`

Use plugins:

`$ vi ~/.zshrc`

```sh
# ...
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

## gtk & qt theme

Install theme and icons:

`# apt install arc-theme papirus-icon-theme lxappearance qt5ct qt5-gtk2-platformtheme`

Setting gtk theme:

`$ lxappearance`

Setting qt theme:

`$ qt5ct`

## fcitx

Install fcitx5:

`# apt install fcitx5 fcitx5-chinese-addons`

Setting fcitx5:

`$ vi ~/.xinitrc`

```sh
# ...
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
fcitx5 &

# Recommend
setxkbmap -option 'ctrl:nocaps'
# ...
```
