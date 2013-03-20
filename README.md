zukuless-live
=============

custom configuration files for Debian Live system (wheezy)

zukuless-live is a set of custom configuration files for the debian
live-build package.
It is already customized for introductory programming with NQC/NXC
(languages for the LEGO mindstorms RIS/NXT), but you can use it as a standard
live system with the xfce4 destop environment. 

## How to build

Your host system to create live images must be a (real or virtual) Debian wheezy machine.

Install "live-build" and "apt-cacher" packages:

    # apt-get install live-build apt-cacher
    # sed -i -e 's/AUTOSTART=0/AUTOSTART=1/g'
    # /etc/init.d/apt-cacher restart

Download the scripts and build:

    $ git clone https://github.com/seijimtmt/zukuless-live.git
    $ cd zukuless-live/
    $ lb config && sudo lb build

Then a live image "binary-YYMMDD.img" will be created.

If necessary, edit "auto/config" before "lb config".
You can set default options for "lb config" (mirror sites, etc.).
For l10n and m17n, see below.

## Copy the image to a USB stick

Check the device name of the USB stick:

    $ ls -l /dev/disk/by-id

If its device name is /dev/sdb, copy the image with dd:

    $ dd if=binary-YYMMDD.img of=/dev/sdb

Be careful because this will overwrite the whole media.

## Usage

You can compile and download your NQC/NXC program to the
RCX/Scout/Spybotics/NXT bricks via the IR tower or a USB cable,
by right-clicking the file icon.

## Localization

Localization (L10N) is quite easy.
You only need to
* edit "auto/config"
* create a local hook that adds your language to boot menu
* add a package list that includes your language task

### Edit auto/config

Set mirrors, flavour, etc. in "auto/config".
Also set ZL_LANGUAGES which is used to include package lists and local hooks
for localization. You can put multiple entries into ZL_LANGUAGES separated
by space, e.g.,

    ZL_LANGUAGES="en fr ja"

or 

    ZL_LANGUAGES="en ja-home ja-class"

### Add boot menu entry

Create a hook with ".binary.ZZ" suffix to add your language to the boot menu, 
where ZZ is one of the elements you've defined in ZL_LANGUAGES. 
For example, "config/hooks/boot-entry.binary.ja" looks like

    #!/bin/sh
    add_boot_entry "japanese" "Live Japanese" \
    "live-config.locales=ja_JP.UTF-8 \
     live-config.timezone=Asia/Tokyo \
     live-config.keyboard-model=jp106 \
     live-config.keyboard-layouts=jp,us"

Here "add_boot_entry" is a local script in local/bin/ that adds a boot
menu entry. It requires three parameters:

    add_boot_entry LABEL MENULABEL APPEND [default]

If the 4th optional parameter "default" is given, the entry will be set as default.

### Add package lists

Put localization list(s) in config/package-lists/ with ".list.chroot.ZZ" suffix.
It is convenient to include task-LANGUAGE and task-LANGUAGE-desktop packages,
which is usually sufficient to prepare your language environment.
For example, 

    echo "task-japanese task-japanese-desktop" > config/package-lists/custom.list.chroot.ja

### Add hooks

Put other localization hooks in config/hooks/ with ".chroot.ZZ"
or ".binary.ZZ" suffix, if necessary. 

### Quick multilingualization (M17N)

Another quick way to enable M17N is to set ZL_LANGUAGES="m17n" in auto/config:

    ZL_LANGUAGES="m17n"

Then add your language entries to "config/hooks/livecfg-m17n.binary.m17n"
and "config/package-lists/custom.list.chroot.m17n", if they are missing.
That's all.

### Note

The localization files "*_l10n-ZZ.list.chroot" (in config/package-lists/),
"*_l10n-ZZ.chroot" and "*_l0n-ZZ.binary" (in config/hooks/), 
which are created automatically by "lb config" (just copied from the files
you add), will be removed by "lb clean". 
