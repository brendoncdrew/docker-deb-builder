In any GNU/Linux distribution, which have a package manager on board, is recommended to install software by using "packages".

Building from source
Step 1. GPG key

The first thing you need is generate a gpg key. 
Package will be signed by this key. 

Run this commands in your terminal:

gpg --gen-key
gpg -a --output ~/.gnupg/bdrew.gpg --export 'Brendon C Drew'
gpg --import ~/.gnupg/bdrew.gpg

It's important to remember that in next stage it will be necessary to use the same name and email,
that were used for creating the key.

Step 2. Prepare the environment

You need install these packages for further building:

sudo apt-get install build-essential autoconf automake autotools-dev \
dh-make debhelper devscripts fakeroot xutils lintian pbuilder

Step 3. Obtain & prepare source code

Download tarball with source code. Unpack it and rename directory like this: "name-version". 
Important: the directory name must be in lowercase.

On the same level with that directory you need to place the archive with the source code. 
You can simply archive a directory that you just created.

mkdir -p ~/build/memcached/1.4.17
cd ~/build/memcached/1.4.17
wget -c http://www.memcached.org/files/memcached-1.4.17.tar.gz
tar -xzf memcached-1.4.17

Step 4. Debianization

Now we will prepare package structure.

cd ~/build/memcached/1.4.17/memcached-1.4.17
dh_make -e youremail@address -f ../memcached-1.4.17.tar.gz

Do not forget that you must use the same email, that was used to generate the key. 
After you run this command in the terminal you'll see the following prompt:

Type of package: single binary, indep binary, multiple binary,
    library, kernel module, kernel patch?
[s /i/m/l/k/n/b]

Choose the easiest option - s

In our directory appeared a new subdirectory called 'debian' which contains the files necessary for further assembly. 
Now we must edit the information about our package: 

>> 'control' file

First of all we must find out dependencies for our package:

dpkg-depcheck -d ./configure

In terminal we will see something like that:

Packages needed:
    mime-support
    libsigsegv2:amd64
    gawk
    libevent-dev

After that we must edit debian/control file like this:

Source: memcached
Section: web
Priority: optional
Maintainer: YOUR NAME <youremail.org>
Build-Depends: debhelper (> = 8.0.0), autotools-dev, mime-support, libsigsegv2, gawk, libevent-dev
Standards-Version: 3.9.4
Homepage: http://memcached.org/

Package: memcached
Architecture: any
Depends: $ {shlibs: Depends}, $ {misc: Depends}
Description: High-performance, distributed memory object caching system
Memcached is an in-memory key-value store for small chunks of arbitrary data (strings, objects)

>> 'copyright' file

In the same "debian" folder, you will also find a "copyright" file which you need to edit. 
The important things to add to this file are the place you got the package from and the actual copyright notice and license.

>> 'changelog' file

Make sure that email used in this file is correct (it must be same as used for gpg-key generation)

Step 5. Build a package

Ok. It's time to build our package. Let's do it:

dpkg-buildpackage -rfakeroot

If there had been no error in the previous stages the assembly process will receive a password prompt from the gpg-key. 
After that our new package will appear in directory one level up. 

You can install it by following command:

sudo dpkg -i memcached_1.4.17-1_amd64.deb

As you can see the assembly was carried out on the platform x86_64. Do you need another platform? No problem.

Assembly for a another platform

To build, for example, for the i386 platform we can use pbuilder - 
an automatic Debian Package Building system for personal development workstation environments.

cd ~/build/memcached/1.4.17
sudo pbuilder --create --architecture i386
sudo pbuilder --update
# memcached_1.4.17-1.dsc was generated when we had built a package
sudo pbuilder --build memcached_1.4.17-1.dsc

Package for i386 platform will be generated in the /var/cache/pbuilder/result directory.

Questions? Mail me: deface@posteo.de

EXAMPLE:
sudo -sH

apt-get install sudo mc build-essential 
apt-get install autoconf automake
apt-get install autotools-dev dh-make debhelper 
apt-get install devscripts fakeroot xutils lintian pbuilder libssh-dev intltool

#gpg --gen-key
#gpg -a --output ~/.gnupg/oleg.pahl.gpg --export 'OLEG PAHL'
#gpg --import ~/.gnupg/oleg.pahl.gpg

mkdir -p ~/build/remmina/1.1.2
cd ~/build/remmina/1.1.2
wget http://archive.ubuntu.com/ubuntu/pool/main/r/remmina/remmina_1.1.2-4ubuntu1.dsc
wget http://archive.ubuntu.com/ubuntu/pool/main/r/remmina/remmina_1.1.2.orig.tar.gz
wget http://archive.ubuntu.com/ubuntu/pool/main/r/remmina/remmina_1.1.2-4ubuntu1.debian.tar.xz

tar -xzf remmina_1.1.2.orig.tar.gz
tar xf remmina_1.1.2-4ubuntu1.debian.tar.xz

mv Remmina-1.1.2 remmina-1.1.2
cd ~/build/remmina/1.1.2/remmina_1.1.2/
#dh_make -e oleg.pahl@icloud.com -f ../remmina_1.1.3.orig.tar.gz - NAVARNO NE NADO!

rm -rf remmina-1.1.2/cmake/FindAVAHI.cmake

cd debian/
echo "DEL ACAHI"
nano control
cd ..

cd remmina-1.1.2/
nano CMakeLists.txt
find_suggested_package(AVAHI) - delete
if(AVAHI_FOUND)
        add_definitions(-DHAVE_LIBAVAHI_UI)
        add_definitions(-DHAVE_LIBAVAHI_CLIENT)
endif()

cd ~/build/remmina/1.1.2/

apt-get install cmake debhelper libfreerdp-dev libgcrypt20-dev libgnome-keyring-dev libgtk-3-dev libjpeg-dev

dpkg-source --commit --include-removal
dpkg-buildpackage -rfakeroot -us -uc


---


#externer.dl.pahl@muenchen.de 
sudo -sH

apt-ge	update
apt-get upgrade
apt-get autoremove
reboot

#rm -rf /etc/apt/sources.list
#rm -rf /etc/apt/sources.list.d/*
#echo "deb http://de.archive.ubuntu.com/ubuntu/ zesty main" > /etc/apt/sources.list
#echo "deb http://de.archive.ubuntu.com/ubuntu/ zesty-security main" >> /etc/apt/sources.list
#apt-get update
#apt-get soft-upgrade

#Welcome to Ubuntu 17.04 (GNU/Linux 4.10.0-37-generic i686)
#
# * Documentation:  https://help.ubuntu.com
# * Management:     https://landscape.canonical.com
# * Support:        https://ubuntu.com/advantage
#
# * CVE-2017-14106 (divide-by-zero vulnerability in the Linux TCP stack) is
#   now fixed in Ubuntu.  Livepatch now, reboot later.
#   - https://ubu.one/CVE-2017-14106
#   - https://ubu.one/rebootLater
#
# 0 packages can be updated.
# 0 updates are security updates.

sleep 3

apt-get install -y sudo mc build-essential 
apt-get install -y autoconf automake
apt-get install -y autotools-dev dh-make debhelper 
apt-get install -y devscripts fakeroot xutils lintian pbuilder intltool
apt-get install -y libssh-dev

#GENERATOR GPG KEYS
#gpg --gen-key
#gpg -a --output ~/.gnupg/oleg.pahl.gpg --export 'OLEG PAHL'
#gpg --import ~/.gnupg/oleg.pahl.gpg
#gpg --list-secret-keys --keyid-format LONG

#I NEED SRC OF REMMINA
mkdir -p ~/build/remmina/1.1.2
cd ~/build/remmina/1.1.2

#pwd
#/root/build/remmina/1.1.2

wget http://archive.ubuntu.com/ubuntu/pool/main/r/remmina/remmina_1.1.2-4ubuntu1.dsc --proxy=off
wget http://archive.ubuntu.com/ubuntu/pool/main/r/remmina/remmina_1.1.2.orig.tar.gz --proxy=off
wget http://archive.ubuntu.com/ubuntu/pool/main/r/remmina/remmina_1.1.2-4ubuntu1.debian.tar.xz --proxy=off

#UNPACK
tar -xzf remmina_1.1.2.orig.tar.gz
tar xf remmina_1.1.2-4ubuntu1.debian.tar.xz

#RENAME BECOUSE OF MANUAL=)
mv Remmina-1.1.2 remmina-1.1.2

#cd ~/build/remmina/1.1.2/remmina-1.1.2/
#dh_make -e oleg.pahl@icloud.com -f ../remmina_1.1.3.orig.tar.gz

#DELETE CMAKE OF REMMINA AVAHI LIB
rm -rf ~/build/remmina/1.1.2/remmina-1.1.2/cmake/FindAVAHI.cmake

#DELETE DECLARATION OF CMAKE
sed -i.bak '/libavahi-ui-gtk3-dev,/d' ~/build/remmina/1.1.2/debian/control
sed -i.bak '/find_suggested_package(AVAHI)/d' ~/build/remmina/1.1.2/remmina-1.1.2/CMakeLists.txt
sed -i '117,121d' ~/build/remmina/1.1.2/remmina-1.1.2/CMakeLists.txt

# INSTALL DEPENDS FOR PKG (WITHOUT AVAHI.LIB)
apt-get install -y cmake debhelper 
apt-get install -y libgcrypt20-dev libgnome-keyring-dev libjpeg-dev
apt-get install -y libgnutls28-dev libtelepathy-glib-dev
apt-get install -y libgtk-3-dev
apt-get install -y libfreerdp-dev libssh-dev libvncserver-dev libvte-2.91-dev libxkbfile-dev libappindicator3-dev
apt-get install -f libavahi-ui-gtk3-dev
apt --fix-broken install


#DONT USE IT!!!
#dpkg-source --commit --include-removal
#-US / -UC = without GPG
dpkg-buildpackage -b -rfakeroot -us -uc

cd ~/build/remmina/1.1.2
pbuilder --create --architecture i386
pbuilder --update
pbuilder --build remmina_1.1.2-4ubuntu1.dsc
sleep 5
dpkg -l | grep -i libavah*
sleep 3
echo "DONE! PLEASE CHECK IT!"
ls -la /var/cache/pbuilder/result

