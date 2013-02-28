zukuless-live
=============

custom configuration files for Debian Live system (wheezy)

The system is customized for introductory programming with NQC/NXC
(languages for the LEGO mindstorms RIS/NXT), but you can use it as a standard
live system with the xfce4 destop environment. 
Currently English and Japanese are supported.

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

## Copy the image to a USB stick

Check the device name of the USB stick:

    $ ls -l /dev/disk/by-id

If its device name is /dev/sdb, copy the image with dd:

    $ dd if=binary-YYMMDD.img of=/dev/sdb

Be careful because this will overwrite the whole media.

## Localization

Save package-list files in config/package-lists/ 
with a ".list.chroot.ZZ" suffix, where ZZ is one of the elements
defined in ${LB_CUSTOM_LANGUAGES} in auto/config. 
For example, if you have "config/package-lists/foo.list.chroot.ja", 
it will be copied to "config/package-lists/foo_l10n-ja.list.chroot"
automatically.

Similarly, save local hooks in config/hooks/
with a ".chroot.ZZ" suffix, e.g., "config/hooks/foo.chroot.ja".
It will be copied to "config/hooks/foo_l10n-ja.chroot" automatically.

"config/package-lists/*_l10n-*list.chroot" and "config/hooks/*_l10n-*.chroot"
will be removed by "lb clean".
