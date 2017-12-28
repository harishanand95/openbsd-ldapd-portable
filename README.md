# openbsd-ldapd-portable

A portable version of the ldapd in OpenBSD. This was tested in ubuntu 16.04

Thanks to Martin Hedenfalk (http://www.bzero.se/ldapd/)

### Installation

Package requirements

    $ apt install git dh-autoreconf byacc flex libssl-dev ldap-utils libbsd-dev 
    
Build Libevent1.4.13 from source
    
    $ wget https://github.com/libevent/libevent/archive/release-1.4.13-stable.tar.gz
    $ tar -xzvf release-1.4.13-stable.tar.gz
    $ cd libevent-release-1.4.13-stable
    $ ./configure
    $ make
    $ sudo make install

Download openbsd-ldapd-portable

    $ git clone https://github.com/harishanand95/openbsd-ldapd-portable.git
    $ cd openbsd-ldapd-portable
    $ ./bootstrap
    $ ./configure
    $ make
    $ make install
    $ sudo ln -s /usr/local/lib/libevent-1.4.so.2 /usr/lib/libevent-1.4.so.2

LDAP specific setup (http://www.bzero.se/ldapd/)

    $ useradd -c "LDAP daemon" -d /var/db/ldap -s /sbin/nologin _ldapd
    $ mkdir -p /var/db/ldap
    $ chown _ldapd /var/db/ldap

OpenBSD LDAP requires a directory `/etc/ldap/` with following files:
* [nis.schema](https://raw.githubusercontent.com/harishanand95/openbsd-ldapd-portable/master/ldapd/schema/nis.schema)
* [inetorgperson.schema](https://raw.githubusercontent.com/harishanand95/openbsd-ldapd-portable/master/ldapd/schema/inetorgperson.schema)
* [core.schema](https://raw.githubusercontent.com/harishanand95/openbsd-ldapd-portable/master/ldapd/schema/core.schema)

It also requires a [/etc/ldapd.conf](https://raw.githubusercontent.com/lattera/openbsd/master/etc/ldapd.conf) with rw permissions for owner only. 
