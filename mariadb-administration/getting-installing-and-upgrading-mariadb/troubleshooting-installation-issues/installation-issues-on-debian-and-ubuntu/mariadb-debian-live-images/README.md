# MariaDB Debian Live Images

<strong>Note:</strong> This page is obsolete. The information is old, outdated, or otherwise currently incorrect. We are keeping the page for historical reasons only. <strong>Do not</strong> rely on the information in this article.

A member of the MariaDB community, Mark `&lt;ms (at) it-infrastrukturen (dot) org&gt;`,
has created some Debian "squeeze" 6.0.4 based, live iso images with [MariaDB 5.2](/kb/en/what-is-mariadb-52/)
or 5.3 pre-installed on them and some Debian "squeeze" 6.0.5 based, live iso images with [MariaDB 5.5](/kb/en/what-is-mariadb-55/) pre-installed on them.

These live and install images can be used to quickly run a MariaDB server in
live mode for learning or testing purposes, or to simplify and speed up
off-line installations of Debian-based MariaDB servers onto harddisk.

To work in live mode the system boots from a usb stick (or CD/DVD) and runs in
RAM without touching the system's harddisk drive.

The same usb stick (or CD/DVD media) can be used to install a complete server
installation onto the harddisk drive using the included Debian installer.

All required MariaDB packages are included on the media, so there is no need
for an Internet connection.

Three types of images are provided, text (command line), LXDE, and Gnome. The
text-based live images can be used for testing or server off-line
installations. The two gui types, LXDE and Gnome, can be used for
testing/learning in live mode or for off-line desktop installations.  Debian
live images with LXDE (gnome, KDE or awesome) are pretty comfortable for daily
work as a replacement for whatever desktop OS is installed on on the system.

There are three iso images for each type, one for 32-bit (i386) systems, one
for 64-bit (amd64) systems, and one with both.

1 [MariaDB 5.2](/kb/en/what-is-mariadb-52/) Live iso images (text) for i386, amd64  or multi architectures.
<ul start="1" style="list-style: none"><li><a undefined>binary-hybrid-squeeze-i386-mariadb52-text.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb52-text.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-i386-amd64-mariadb52-text.iso</a>
</li></ul>

1 [MariaDB 5.3](/kb/en/what-is-mariadb-53/) Live iso images (text) for i386, amd64  or multi architectures.
<ul start="1" style="list-style: none"><li><a undefined>binary-hybrid-squeeze-i386-mariadb53-text.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb53-text.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-i386-amd64-mariadb53-text.iso</a>
</li></ul>

1 [MariaDB 5.3](/kb/en/what-is-mariadb-53/) Live iso images with LXDE for i386, amd64  or multi architectures.
<ul start="1" style="list-style: none"><li><a undefined>binary-hybrid-squeeze-i386-mariadb53-lxde.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb53-lxde.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-i386-amd64-mariadb53-lxde.iso</a>
</li></ul>

1 [MariaDB 5.3](/kb/en/what-is-mariadb-53/) Live iso images with Gnome for i386, amd64  or multi architectures.
<ul start="1" style="list-style: none"><li><a undefined>binary-hybrid-squeeze-i386-mariadb53-gnome.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb53-gnome.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-i386-amd64-mariadb53-gnome.iso</a>
</li></ul>

1 [MariaDB 5.5](/kb/en/what-is-mariadb-55/) Live images
<ul start="1" style="list-style: none"><li><a undefined>binary-hybrid-squeeze-amd64-mariadb55-text-bpo.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb55-lxde-bpo.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb55-gnome-bpo.iso</a>
</li><li><a undefined>binary-hybrid-squeeze-amd64-mariadb55-awesome-bpo.iso</a>
</li></ul>

1 [MariaDB 5.5](/kb/en/what-is-mariadb-55/) Live images demonstration video
<ul start="1" style="list-style: none"><li><a undefined>README-mariadb-video.txt</a>
</li><li><a undefined>video-mariadb5.5-live-images-on-USB.ogv</a>
</li><li><a undefined>video-mariadb5.5-live-images-on-USB.mp4</a>
</li></ul>

The LXDE and Gnome images contain documentation under `/srv/PDF`. Including
instructions on how to create your own Debian live images in live mode (you
need 16GB RAM or more to be able to do this). See the README, README.live, and
live-manual.en.pdf files under `/srv/PDF` for details.

<strong>Note:</strong> Some HP notebooks are not able to boot binary hybrid iso images from a USB stick.

## Downloads

To get the iso images you can use rsync:

```sql
rsync -avP rsync://rsync.it-infrastrukturen.org/ftp/public-mariadb/<image_or_file_name_as_above> .
```

or just use the links above (or go to: [http://rsync.it-infrastrukturen.org/public-mariadb/](http://rsync.it-infrastrukturen.org/public-mariadb/) ).

## Preparing of bootable media:

A USB stick or CD/DVD can be used as bootable media. The preferred way is a USB stick.

- for Linux: `dd if=./&lt;image_name_as_above.iso&gt; of=/dev/&lt;USB-stick-ID_like_sdb_or_sdc&gt;`
- for other systems: cygwin includes dd and some other linux/Unix tools.

The iso images have been successfully tested on:

- Acer ASPIRE @ne netbooks (1GB RAM)
- ThinkPad T60p and T61p notebooks (2 or 4GB RAM)
- Xeon E31270 / Asus P8BWS desktop (16GB ECC RAM)
- AMD FX-4100 / Asus M5A99X EVO desktop (16GB ECC RAM)
- HP DL385g7 Opteron 6170SE or 6180SE servers (32 or 64GB ECC RAM)

### Notes

It is possible to add some options on the bootline for special purposes like,
for example, the installation of additional packages from a local repository on
the image or assigning a static IP address and running sshd.

1 To configure a static IP use "`ip=`". For example:
    "`ip=eth0:192.168.1.53:255.255.255.0:192.168.1.1`"
<ul start="1" style="list-style: none"><li>The full format of "`ip=`" is: "`ip=[interface]:[IP_address]:[netmask]:[gateway]`"
</li></ul>

1 If you use an empty "`ip=`" value the content of `/etc/network/interfaces`
    and `/etc/resolv.conf` will be used. Without the "`ip=`" option, dhcp
    will be used to get an IP address.

1 For installing and running sshd (so that the image can act as a server in a
    test environment) use: "`sshd=on`"
<ul start="1" style="list-style: none"><li>The live user is "sql" and the password is "live". Please change the
       password immediately after first login!
</li></ul>

1 Depending on the image [MariaDB 5.2](/kb/en/what-is-mariadb-52/) or 5.3 server is installed. There is no
    root passord for the database set so please set a password immediately
    after first login!

1 Run `repo-off-line.sh` to activate local repositories on the image for
    apt-get (for off-line installations) or `repo-on-line.sh` for internet
    repositories.

1 The binary hybrid live iso images with multiple-choices inside the boot
    menu allow for the assignment of two different IPs. (e.g. for testing of
    clustered database operations)

Send any feedback or suggestions related to these images to:

- Mark `&lt;ms (at) it-infrastrukturen (dot) org&gt;`