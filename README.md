# My Gentoo Configuration & Dependencies
The following are Gentoo Linux configuration personalised by me.

## Motivation
"Building" an minimal, clean and optimum OS for development purposes.

### Kernel Configuration
- HID over I2C transport layer ACPI driver [*] *(for Elantech touchpad)*
- Macintosh device drivers [ ]
- Reiserfs, JFS, XFS, GFS2, OCFS2, Btrfs, NILFS2, F2FS, zonefs [ ]
- ATI Radeon, AMD GPU, Nouveau cards [ ]
- MMC/SD/SDIO card support [ ]
- Sony MemoryStick card support [ ]
- Disable joystick, tablets, touchscreen, [ ]

### /etc/portage/make.conf
```
USE="X pulseaudio elogind postproc text -bindst -ldap -openldap -gnome -systemd -kde -css -bluetooth -multilib -qt -qt5 -qtwebengine -webengine -wayland"
INPUT_DEVICES="libinput synaptics"
VIDEO_CARDS="intel"
MAKEOPTS="-j5"
ACCEPT_LICENSE="*"
GRUB_PLATFORM="efi-64"
PHP_TARGETS="php8-1 php7-4"
NGINX_MODULE_HTTP="fastcgi"

```

### Packages
- app-editors/vim
- app-editors/vscode
- dev-db/mariadb
- dev-lang/php-8.1.16
- dev-lang/php-7.4.33
- dev-php/pecl-imagick
- dev-vcs/git
- mail-client/thunderbird
- media-fonts/noto
- media-fonts/noto-cjk
- media-fonts/noto-emoji
- media-fonts/ubuntu-font-family
- media-gfx/feh
- net-im/discord ---> Can be installed with tar package into /opt/discord and insert dmenu at /usr/local/bin
- net-misc/freerdp
- net-vpn/openvpn
- net-wireless/iwd
- sys-boot/grub
- sys-firmware/intel-microcode
- sys-power/acpilight
- sys-power/acpi ---> Need to give permission to acpilight
- virtual/cron
- www-client/firefox
- www-servers/nginx
- x11-apps/xrandr
- x11-base/xorg-server
- x11-libs/libnotify
- x11-misc/cbatticon
- x11-misc/i3lock
- x11-misc/i3status
- x11-terms/st
- x11-wm/i3

### /etc/portage/package.use
```
media-libs/libsndfile minimal
app-crypt/gcr gtk
app-text/ghostscript-gpl cups
dev-lang/php -opcache gd mysql pdo mysqli fpm cli intl curl calendar zip xmlreader xmlwriter
app-eselect/eselect-php fpm
app-misc/mime-types nginx
*/* PYTHON_TARGETS: -* python3_11
*/* PYTHON_SINGLE_TARGET: -* python3_11
```

## Manual Kernel Configuration ##
##### Please enable framebuffer first! #####
> - Device Drivers
>   - Graphic Supports
>     - Frame buffer Devices
>       - <*> Support for frame buffer device drivers
>

##### Synaptics Touchpad #####
> - Device Drivers
>   - Input device support
>     - Mice
>       - [*] Elantech PS/2 protocol extension

##### Sound #####
> - Device Drivers
>   - Sound card support
>     - ALSA
>       - [*] Build Realtek HD-audio codec support
>       - [*] Build Analog Devices HD-audio codec support
>       - [*] Build IDT/Sigmatel HD-audio codec support
>       - [*] Build VIA HD-audio codec support
>       - [*] Build HDMI/DisplayPort HD-audio codec support

## Problems Encountered

##### Unable to startx after configuring .xinitrc
  > Make sure to add the user into the **video** group

##### Touchpad not working (elantech)
  > Enable **HID over I2C transport layer ACPI driver** inside kernel configuration and recompile.

##### Unable to play sound
  > Make sure the .config folder is writeable. Use **pulsemixer** to check if the output is muted or not.
  > Add the user to **audio** group.

##### Unable to change screen brightness
  > Bunch of solution online, emerge sys-power/acpilight and try with command **xbacklight -dec 2**, it usually works.

##### Installed apache and php-fpm, but php files showing blank page (PHP-FPM through mod_proxy_fcgi method)
  > Follow the instruction given in the handbook.
  > Add **APACHE2_MODULES="$APACHE2_MODULES http2 proxy proxy_fcgi"** into your **make.conf** and update.

##### Kernel 6.1.38 high temperature when idle #####
  > Enabling Laptop Hybrid Graphics and Nouveau (NVIDIA) cards modules.
  > Follow how to power down graphic card on hybrid graphics wiki in Gentoo handbook.

##### Unable to run Zoom-5.15.3.4839.glibc2.17-x86_64.AppImage #####
  > Emerge app-crypt/mit-krb5.


## I F*ed up! What should I do? ##
If your grub is gone, boot into the live cd and ssh into the live cd. Copy all the instructions given below.

```
mount --types proc /proc /mnt/gentoo/proc && mount --rbind /sys /mnt/gentoo/sys && mount --make-rslave /mnt/gentoo/sys && mount --rbind /dev /mnt/gentoo/dev && mount --make-rslave /mnt/gentoo/dev && mount --bind /run /mnt/gentoo/run && mount --make-slave /mnt/gentoo/run 
```

```
chroot /mnt/gentoo /bin/bash  && source /etc/profile && export PS1="(chroot) ${PS1}"
```
The command above will chroot you into your system. You may reinstall the kernel if you like to.

```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Gentoo

grub-mkconfig -o /boot/grub/grub.cfg
```
The command above will reinstall grub. Sometimes you don't need to reinstall grub, you only need to reconfigure the grub.

## Contributing
I'm not a Linux expert nor a skilled programmer, feel free to help me in "building" optimised, clean and minimal linux environment by creating an issue or PR.

## What is next?
Linux From Scratch?
