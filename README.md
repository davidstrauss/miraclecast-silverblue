# Miraclecast on Fedora Silverblue

*This doesn't work yet.*

1. Create and enter a toolbox:

       toolbox create
       toolbox enter
       sudo dnf upgrade -y

1. From inside the toolbox, build and install:

       sudo dnf install autoconf gcc make automake libtool readline-devel glib2-devel systemd-devel
       git clone https://github.com/albfan/miraclecast.git
       cd miraclecast
       ./autogen.sh
       ./configure CFLAGS='-g -O0 -ftrapv' --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib64
       make
       mkdir installroot
       make DESTDIR="$HOME/miraclecast/installroot" install

1. From outside the toolbox, copy over the D-Bus policy:

       sudo cp ~/miraclecast/installroot/etc/dbus-1/system.d/org.freedesktop.miracle.conf /etc/dbus-1/system.d/

1. From outside the toolbox, stop existing network services and launch the Miraclecast daemon:

       sudo systemctl stop NetworkManager.service
       sudo systemctl stop wpa_supplicant.service
       sudo ~/miraclecast/installroot/usr/bin/miracle-wifid
       
1. Also from outside the toolbox, start the Miraclecast controller:

       sudo ~/miraclecast/installroot/usr/bin/miracle-sinkctl

## Notes

* File `src/dhcp/dhcp.c` defaults to using `/bin/ip` when Fedora needs it to use `/usr/sbin/ip`. Otherwise, it will fail to issue an IP address to the peer.
* The contents of `~/miraclecast/installroot/usr/bin/` must be in `PATH`, possibly for both utilities. Otherwise, wpa_supplicant will fail to load.
