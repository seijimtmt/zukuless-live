zukuless-live
=============

custom configuration files for Debian Live system (wheezy)

zukuless-live is a set of custom configuration files for the debian
live-build package.
It is already customized for introductory programming with NQC/NXC
(languages for the LEGO mindstorms RIS/NXT), but you can use it as a standard
live system with the xfce4 destop environment. 

## How to build

Your host system to create live images must be a Debian wheezy machine.

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
You can set default language ("en" or "ja") and/or options for
"lb config" (keyboard type, mirror sites, etc.).
See also the Localization section below.

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

Although only English and Japanese are currenty supported, it's easy
to add other languages.
Basically you only need to edit "auto/config" and to add package lists
and hooks.

### Edit auto/config

Set two variables, LB_CUSTOM_DEFAULT_LANGUAGE and LB_CUSTOM_LANGUAGES.
The former is used for "lb config", while the latter
for including package lists and local hooks for localization.
You can put multiple values to LB_CUSTOM_LANGUAGES separated by space.
e.g.,

    LB_CUSTOM_DEFAULT_LANGUAGE="ja"
    LB_CUSTOM_LANGUAGES="ja en"

Add options to "lb config" for your default language, by using the "ja"
case as reference.

### Add package lists

Put localizatoin lists in config/package-lists/ with ".list.chroot.ZZ" suffix,
where ZZ is one of the elements you've defined in LB_CUSTOM_LANGUAGES
(e.g., foo.list.chroot.ja).

### Add hooks

Put localization hooks in config/hooks/ with ".chroot.ZZ" suffix, 
where ZZ is one of the elements you've defined in LB_CUSTOM_LANGUAGES
(e.g., foo.chroot.ja).

### Note

The localization files "*_l10n-ZZ.list.chroot" (in config/package-lists/)
and "*_l10n-ZZ.chroot" (in config/hooks/), which are created automatically
by "lb config" (just copied from the files you add), 
will be removed by "lb clean".
