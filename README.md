# Miraclecast on Fedora Silverblue

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
