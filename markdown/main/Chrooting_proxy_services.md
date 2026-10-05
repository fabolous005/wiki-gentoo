<!-- source: https://wiki.gentoo.org/wiki/Chrooting_proxy_services | group: Gentoo Wiki (Main) | wiki-title: Chrooting proxy services -->
---
title: Chrooting proxy services
url: https://wiki.gentoo.org/wiki/Chrooting_proxy_services
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-14"
categories: ['some privacy addons']
fingerprint: "8438916ff7267967"
license: CC BY-SA 4.0
---

# Chrooting proxy services

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Today there are many process isolation techniques. Most of them are based on virtualization or containers. Some are focused on security, which is what we want for this.

## Kernel options

*use hardened chroot*to isolate internet services, but since

**hardened-sources**is no longer available and there is no replacement for most of us, therefore we will do without. We leave this section in for those that can still use it.

To create a *hardened jail* we need **hardened-sources** to be installed (it is wise to use one of hardened profiles). So emerge it:

`root #``emerge --ask sys-kernel/hardened-sources`
Then set the necessary hardened chroot options:

**make menuconfig options**

## Chroot

As an example, of building chroot services, lets take a look at home proxy server. A home proxy can look something like:

```
                  +-------------------------------------------------------------+
                  | Chrooted sockd or torsocks <-> Other Internet applications  |
                  |      ^                                                      |
                  |      |                                                      |
                  |                                Chrooted          HTTP*      |
 +----------+     |  Chrooted  <->  Chrooted  <->    HAVP    <->    Internet    |
 | Internet | <-> | <-> Tor         Privoxy            +          applications  |
 +----------+     |                    ^           libClamAV                    |
                  |                    |                                        |
                  |                                                             |
                  |                 Chrooted                                    |
                  |                 FreshClam                                   |
                  +-------------------------------------------------------------+
```
From a users perspective the best way would be to write an ebuild to build the chroot of the service!!! So generally for a chrooted tor service the Gentoo user wants to run:

`root #``emerge --config net-misc/tor`
and that is all...
Except developers don't want to support such a complicated ebuild. Therefore, here we will show examples of chrooted init scripts for all services shown above and examples of bash scripts to build the chroots (these should be hooked into the *pkg\_config()* function of the respective ebuilds).

First **build and install binary packages** for the services [ClamAV](https://wiki.gentoo.org/wiki/ClamAV), [tor](https://wiki.gentoo.org/wiki/Tor), [Dante](http://packages.gentoo.org/packages/net-proxy/dante), [HAVP](http://packages.gentoo.org/packages/net-proxy/havp) and [privoxy](https://wiki.gentoo.org/wiki/Privoxy):

`root #``emerge --ask -b app-antivirus/clamav net-vpn/tor net-proxy/torsocks net-proxy/dante net-proxy/havp net-proxy/privoxy`
Then configure all of them, which is beyond the scope of this how-to, though. However, for this setup to work we need `USE=clamav` on HAVP.

The next scripts build chrooted services even when *all file-systems with executables are mounted readonly and all writeable file-systems are mounted with noexec*. Make sure you have write-access to the / and /usr partitions when you execute these!

**You must manually run the chroot build scripts any time you update or reconfigure the service or update this library!**

### Chrooted FreshClamAV

**`/etc/init.d/clamd`**

**chrooted freshclam**

```
#!/sbin/openrc-run
# Copyright 1999-2015 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
CHROOT="/usr/chroot/clamav"
PIDCLAMD="/var/run/clamav/clamd.pid"
PIDFRESH="/var/run/clamav/freshclam.pid"
daemon_clamd="/usr/sbin/clamd"
daemon_freshclam="/usr/bin/freshclam"
daemon_milter="/usr/sbin/clamav-milter"
extra_commands="logfix"
depend() {
        use net
        provide antivirus
}
get_config() {
        clamconf | sed 's/["=]//g' | \
        awk "{
        if(\$0==\"Config file: $1.conf\") S=1
        if(S==1&&\$0==\"\") {
                print \"$3\"
                exit
        }
        if(S==1&&\$1~\"^$2\$\") {
                print \$2!=\"disabled\"?\$2:\"$3\"
                exit
        }
        }"
}
checkconfig() {
        checkpath --directory \
                --owner "${CLAMAV_USER:-clamav}:${CLAMAV_GROUP:-clamav}" \
                --mode 0755 --directory `dirname ${CHROOT}${PIDCLAMD}`
        if [ ! -c ${CHROOT}/dev/null ] ; then
                mknod -m 666 ${CHROOT}/dev/null c 1 3
                mount -ro remount ${CHROOT}/dev
        fi
}
start() {
        # populate variables and fix log file permissions
        logfix
        if [ "${START_CLAMD}" = "yes" ]; then
                checkconfig || return 1
                if [ -S "${CHROOT}${clamd_socket}" ]; then
                        rm -f ${CHROOT}${clamd_socket}
                fi
                ebegin "Starting chrooted clamd"
                start-stop-daemon --start --quiet \
                        --nicelevel ${CLAMD_NICELEVEL:-0} \
                        --ionice ${IONICE_LEVEL:-0} \
                        --pidfile "${CHROOT}${PIDCLAMD}" \
                        --exec chroot ${CHROOT} ${daemon_clamd}
                eend $? "Failed to start clamd"
        fi
        if [ "${START_FRESHCLAM}" = "yes" ]; then
                checkconfig || return 1
                ebegin "Starting chrooted freshclam"
                start-stop-daemon --start --quiet \
                        --nicelevel ${FRESHCLAM_NICELEVEL:-0} \
                        --ionice ${IONICE_LEVEL:-0} \
                        --pidfile "${CHROOT}${PIDFRESH}" \
                        --exec chroot ${CHROOT} ${daemon_freshclam} -- -d --pid "${PIDFRESH}"
                retcode=$?
                if [ ${retcode} = 1 ]; then
                        eend 0
                        einfo "Virus databases are already up to date."
                else
                        eend ${retcode} "Failed to start freshclam"
                fi
        fi
        if [ "${START_MILTER}" = "yes" ]; then
                if [ -z "${MILTER_CONF_FILE}" ]; then
                        MILTER_CONF_FILE="/etc/clamav-milter.conf"
                fi
                ebegin "Starting clamav-milter"
                start-stop-daemon --start --quiet \
                        --nicelevel ${MILTER_NICELEVEL:-0} \
                        --ionice ${IONICE_LEVEL:-0} \
                        --exec ${daemon_milter} -- -c ${MILTER_CONF_FILE}
                eend $? "Failed to start clamav-milter"
        fi
}
stop() {
        if [ "${START_CLAMD}" = "yes" ]; then
                ebegin "Stopping chrooted clamd"
                start-stop-daemon --stop --pidfile "${CHROOT}${PIDCLAMD}" --quiet --name clamd
                eend $? "Failed to stop clamd"
        fi
        if [ "${START_FRESHCLAM}" = "yes" ]; then
                ebegin "Stopping chrooted freshclam"
                start-stop-daemon --stop --quiet \
                --pidfile "${CHROOT}${PIDFRESH}" \
                --name freshclam \
                --exec chroot ${CHROOT} ${daemon_freshclam} -- -K --pid "${PIDFRESH}"
                eend $? "Failed to stop freshclam"
        fi
        if [ "${START_MILTER}" = "yes" ]; then
                ebegin "Stopping clamav-milter"
                start-stop-daemon --stop --quiet --name clamav-milter
                eend $? "Failed to stop clamav-milter"
        fi
}
logfix() {
        clamd_socket=$(get_config clamd LocalSocket /run/clamav/clamd.sock)
        clamd_user=$(get_config clamd User clamav)
        freshclam_user=$(get_config freshclam DatabaseOwner clamav)
        if [ "${START_CLAMD}" = "yes" ]; then
                # fix clamd log permissions
                # (might be clobbered by logrotate or something)
                local logfile=$(get_config clamd LogFile)
                if [ -n "${CHROOT}${logfile}" ]; then
                        checkpath --quiet \
                                --owner "${clamd_user}":"${clamd_user}" \
                                --mode 640 \
                                --file ${CHROOT}${logfile}
                fi
        fi
        if [ "${START_FRESHCLAM}" = "yes" ]; then
                # fix freshclam log permissions
                # (might be clobbered by logrotate or something)
                local logfile=$(get_config freshclam UpdateLogFile)
                if [ -n "${CHROOT}${logfile}" ]; then
                        checkpath --quiet \
                                --owner "${freshclam_user}":"${freshclam_user}" \
                                --mode 640 \
                                --file ${CHROOT}${logfile}
                fi
        fi
}
```
**`clamav-chroot.sh`**

**Build chrooted freshclam**

```
#!/bin/bash
# 20150922
# GPL-3
PKGDIR="/var/cache/binpkgs"
CATEGORY="app-antivirus"
PN="clamav"
CHROOT="/usr/chroot/${PN}"
WORKD=`pwd`
# Cleaning chroot directory.
umount "${CHROOT}"/var/lib/${PN} "${CHROOT}"/var/log/${PN} "${CHROOT}"/var/run "${CHROOT}"/var/tmp "${CHROOT}"/dev 1>/dev/null 2>&1
rm -rf "${CHROOT}"
# Make common directory and symlinks.
mkdir -p "${CHROOT}"/{dev,etc}
if [ -d /lib64 ]
    then
        mkdir -p "${CHROOT}"/{lib64,usr/lib64}
        cd "${CHROOT}" && ln -s lib64 lib
        cd "${CHROOT}/usr" && ln -s lib64 lib
    else
        mkdir -p "${CHROOT}"/{lib,usr/lib}
fi
mkdir -p /var/log/${PN} "${CHROOT}"/var/log/${PN} "${CHROOT}"/var/run "${CHROOT}"/var/tmp
chown -R ${PN}:${PN} /var/log/${PN} "${CHROOT}"/var/log/${PN}
chmod -R o-rwx /var/log/${PN} "${CHROOT}"/var/log/${PN}
# Extract package.
tar -xjphf `ls ${PKGDIR}/${CATEGORY}/${PN}-0* | tail -n 1` -C "${CHROOT}"
# Copy necessary libraries.
cp -pRPd /lib/ld-* "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/bin/freshclam" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/bin/freshclam" | awk '{print $3}' | grep "^/usr/lib"` "${CHROOT}/usr/lib"
# Copy user information and necessary libraries for it.
cp -pRPd /lib/libnss* /lib/libnsl* /lib/libresolv* "${CHROOT}/lib"
cp /usr/lib/libnss3.so "${CHROOT}/usr/lib"
grep "^${PN}" "/etc/passwd" > "${CHROOT}/etc/passwd"
grep "${PN}" "/etc/group" > "${CHROOT}/etc/group"
# fstab stuff.
if [[ `grep "Clamav chroot stuff." /etc/fstab` == '' ]]
    then
        cat >> /etc/fstab << EOF
# Clamav chroot stuff.
/var/lib/${PN}          ${CHROOT}/var/lib/${PN}         none    bind,nodev,noexec,nosuid,rw                                     0 0
/var/log/${PN}          ${CHROOT}/var/log/${PN}         none    bind,nodev,noexec,nosuid,rw                                     0 0
/var/tmp/havp           ${CHROOT}/var/tmp               none    bind,nodev,noexec,nosuid,user=${PN},ro                          0 0
none                    ${CHROOT}/var/run               tmpfs   rw,nodev,noexec,nosuid,relatime,size=1024k,mode=755             0 0
none                    ${CHROOT}/dev                   tmpfs   rw,noexec,nosuid,relatime,size=1024k,nr_inodes=384443,mode=755  0 0
EOF
fi
mount -a
# Configuration.
cp -fp /etc/freshclam.conf ${CHROOT}/etc/
cp -fp /etc/clamd.conf ${CHROOT}/etc/
cd ${WORKD}
cp -f clamd /etc/init.d/
cp -f clamd ${CHROOT}/etc/init.d/
exit 0
```
### Chrooted Dante Sockd

**`/etc/init.d/dante-sockd`**

**chrooted dante-sockd**

```
#!/sbin/openrc-run
# Copyright 1999-2015 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
# $Id$
CHROOT=/usr/chroot/dante
SOCKD_OPT=""
[ "${SOCKD_FORKDEPTH:-1}" -gt 1 ] && SOCKD_OPT="${SOCKD_OPT} -N ${SOCKD_FORKDEPTH}"
[ "${SOCKD_DEBUG:-0}" -eq 1 ] && SOCKD_OPT="${SOCKD_OPT} -d"
[ "${SOCKD_DISABLE_KEEPALIVE:-0}" -eq 1 ] && SOCKD_OPT="${SOCKD_OPT} -n"
PIDFILE=/var/run/dante/sockd.pid
SOCKDIR=/var/lock/dante-sockd/
depend() {
        need net
}
checkconfig() {
        # first check that it exists
        if [ ! -f ${CHROOT}/etc/socks/sockd.conf ] ; then
                eerror "You need to setup /etc/socks/sockd.conf first"
                eerror "Examples are in /usr/share/doc/dante[version]/example"
                eerror "for more info, see: man sockd.conf"
                return 1
        fi
        /usr/sbin/sockd -V -f ${CHROOT}/etc/socks/sockd.conf >/tmp/dante-sockd.checkconf 2>&1
        if [ $? -ne 0 ]; then
                cat /tmp/dante-sockd.checkconf
                eerror "Something is wrong with your configuration file"
                eerror "for more info, see: man sockd.conf"
                return 1
        fi
        rm /tmp/dante-sockd.checkconf
        DAEMON_UID=`sed -e '/^[ \t]*user[.]notprivileged[ \t]*:/{s/.*:[ \t]*//;q};d' /etc/socks/sockd.conf`
        if [ -n "$DAEMON_UID" ]; then
                checkpath --quiet --mode 755 --owner "${DAEMON_UID}":root --directory `dirname ${CHROOT}${PIDFILE}`
                checkpath --quiet --mode 750 --owner "${DAEMON_UID}":root --directory "${CHROOT}${SOCKDIR}"
        else
                checkpath --quiet --mode 755 --directory `dirname ${CHROOT}${PIDFILE}`
                checkpath --quiet --mode 750 --directory "${CHROOT}${SOCKDIR}"
        fi
        return 0
}
start() {
        checkconfig || return 1
        ebegin "Starting chrooted dante sockd"
        start-stop-daemon --start --quiet \
                --background --pidfile ${CHROOT}$PIDFILE --make-pidfile --env TMPDIR=$SOCKDIR \
                --exec chroot ${CHROOT} /usr/sbin/sockd -- ${SOCKD_OPT} >/dev/null 2>&1
        eend $? "Failed to start sockd"
}
stop() {
        ebegin "Stopping chrooted dante sockd"
        start-stop-daemon --stop --quiet --pidfile ${CHROOT}$PIDFILE
        eend $? "Failed to stop sockd"
}
```
**`dante-chroot.sh`**

**Build chrooted dante-sockd**

```
#!/bin/bash
# 20150922
# GPL-3
PKGDIR="/var/cache/binpkgs"
CATEGORY="net-proxy"
PN="dante"
CHROOT="/usr/chroot/${PN}"
USER="sockd"
WORKD=`pwd`
# Cleaning chroot directory.
umount "${CHROOT}"/var/lock/ "${CHROOT}"/var/log/${PN} "${CHROOT}"/var/run "${CHROOT}"/dev "${CHROOT}"/tmp 1>/dev/null 2>&1
rm -rf "${CHROOT}"
# Make common directory and symlinks.
mkdir -p "${CHROOT}"/{dev,etc}
if [ -d /lib64 ]
    then
        mkdir -p "${CHROOT}"/lib64
        cd "${CHROOT}" && ln -s lib64 lib
    else
        mkdir -p "${CHROOT}"/{lib}
fi
mkdir -p /var/log/${PN} /var/tmp/${PN} "${CHROOT}"/var/log/${PN} "${CHROOT}"/var/lock "${CHROOT}"/var/run "${CHROOT}"/tmp
chown -R ${USER}:root /var/log/${PN} /var/tmp/${PN} "${CHROOT}"/var/log/${PN}
chmod -R o-rwx /var/log/${PN} /var/tmp/${PN} "${CHROOT}"/var/log/${PN}
# Extract package.
tar -xjphf `ls ${PKGDIR}/${CATEGORY}/${PN}* | tail -n 1` -C "${CHROOT}"
# Copy necessary libraries.
cp -pRPd /lib/ld-* "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/sbin/${USER}" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
# Copy user information and necessary libraries for it.
cp -pRPd /lib/libnss* /lib/libnsl* /lib/libresolv* "${CHROOT}/lib"
cp /usr/lib/libnss3.so "${CHROOT}/usr/lib"
grep "^${USER}" "/etc/passwd" > "${CHROOT}/etc/passwd"
# fstab stuff.
if [[ `grep "Dante chroot stuff." /etc/fstab` == '' ]]
    then
        cat >> /etc/fstab << EOF
# Dante chroot stuff.
/var/log/${PN}          ${CHROOT}/var/log/${PN}         none    bind,nodev,noexec,nosuid,rw                                     0 0
/var/tmp/${PN}          ${CHROOT}/tmp                   none    bind,nodev,noexec,nosuid,rw                                     0 0
none                    ${CHROOT}/var/lock              tmpfs   rw,nodev,noexec,nosuid,relatime,size=1024k,mode=755             0 0
none                    ${CHROOT}/var/run               tmpfs   rw,nodev,noexec,nosuid,relatime,size=1024k,mode=755             0 0
EOF
fi
mount -a
# Configuration.
cp -fp /etc/socks/* ${CHROOT}/etc/socks/
cp -fp /etc/conf.d/${PN}-sockd ${CHROOT}/etc/conf.d/
cd ${WORKD}
cp -f ${PN}-sockd /etc/init.d/
cp -f ${PN}-sockd ${CHROOT}/etc/init.d/
exit 0
```
### Chrooted HAVP + libClamAV

**`/etc/init.d/havp`**

**chrooted havp**

```
#!/sbin/openrc-run
# Copyright 1999-2015 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
CHROOT=/usr/chroot/${SVCNAME}
PIDFILE=/var/run/${SVCNAME}/${SVCNAME}.pid
depend() {
#       need net
        use clamd \
#               squid apache2 bfilter mman junkbuster oops polipo privoxy tinyproxy tor wwwoffled
#       #havp could be used in conjuction with any parent proxies enumerated above
}
get_havp_opt() {
        eval HAVP_$1=`awk '/^[ \t]*'$1'[ \t]+/ { print $2; }' < /etc/${SVCNAME}/${SVCNAME}.config`
}
checkconfig() {
        if [ ! -f ${CHROOT}/etc/${SVCNAME}/${SVCNAME}.config ] ; then
                eerror "No ${CHROOT}/etc/${SVCNAME}/${SVCNAME}.config file exists!"
                return 1
        fi
        local HAVP_USER
        get_havp_opt USER
        if [ -n "${HAVP_USER}" ] && ! getent passwd ${HAVP_USER} > /dev/null ; then
                eerror "${HAVP_USER} user is missing!"
                return 1
        fi
        local HAVP_GROUP
        get_havp_opt GROUP
        if [ -n "${HAVP_GROUP}" ] && ! getent group ${HAVP_GROUP} > /dev/null ; then
                eerror "${HAVP_GROUP} group is missing!"
                return 1
        fi
        if [ ! -c ${CHROOT}/dev/null ] ; then
                mknod -m 666 ${CHROOT}/dev/null c 1 3
                mount -ro remount ${CHROOT}/dev
        fi
        checkpath --directory \
            --owner "${HAVP_USER:-havp}:${HAVP_GROUP:-havp}" --mode 0755 --directory `dirname ${CHROOT}${PIDFILE}`
        checkpath --directory \
            --owner "${HAVP_USER:-havp}:${HAVP_GROUP:-havp}" --mode 0700 ${CHROOT}/var/log/havp
        checkpath --directory \
            --owner "${HAVP_USER:-havp}:${HAVP_GROUP:-havp}" --mode 0750 "${CHROOT}/var/tmp/${SVCNAME}"
}
start() {
        checkconfig || return 1
/ and /usr file system when execute it! 
        ebegin "Starting chrooted HTTP AntiVirus Proxy"
        start-stop-daemon --start --pidfile=${CHROOT}${PIDFILE} --exec chroot ${CHROOT} /usr/sbin/${SVCNAME} > /dev/null
        eend $?
}
stop() {
        local HAVP_PIDFILE
        get_havp_opt PIDFILE
        ebegin "Stopping chrooted HTTP AntiVirus Proxy"
        start-stop-daemon --stop --pidfile=${CHROOT}${PIDFILE}
        eend $?
}
```
**`havp-chroot.sh`**

**Build chrooted havp**

```
#!/bin/bash
# 20150922  havp-chroot.sh
# GPL-3
PKGDIR="/var/cache/binpkgs"
CATEGORY="net-proxy"
PN="havp"
CHROOT="/usr/chroot/${PN}"
WORKD=`pwd`
# Cleaning chroot directory.
umount "${CHROOT}"/var/lib/clamav "${CHROOT}/var/log/${PN}" "${CHROOT}"/var/run "${CHROOT}"/var/tmp "${CHROOT}"/dev 1>/dev/null 2>&1
rm -rf "${CHROOT}"
# Make common directory and symlinks.
mkdir -p "${CHROOT}"/{dev,etc}
if [ -d /lib64 ]
    then
        mkdir -p "${CHROOT}"/{lib64,usr/lib64}
        cd "${CHROOT}" && ln -s lib64 lib
        cd "${CHROOT}/usr" && ln -s lib64 lib
    else
        mkdir -p "${CHROOT}"/{lib,usr/lib}
fi
mkdir -p /var/log/"${PN}" "/var/tmp/${PN}" "${CHROOT}"/var/lib/clamav "${CHROOT}/var/log/${PN}" "${CHROOT}"/var/tmp/ "${CHROOT}"/var/run
chown -R ${PN}:${PN} /var/log/${PN} /var/tmp/${PN} "${CHROOT}/var/log/${PN}"
chmod -R o-rwx /var/log/${PN} /var/tmp/${PN} "${CHROOT}/var/log/${PN}"
chmod -R g-rwx /var/log/${PN} "${CHROOT}"/var/log/${PN}
# Extract package.
tar -xjphf `ls ${PKGDIR}/${CATEGORY}/${PN}* | tail -n 1` -C "${CHROOT}"
# Copy necessary libraries.
cp -pRPd /lib/ld-* "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/sbin/${PN}" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/sbin/${PN}" | awk '{print $3}' | grep "^/usr/lib"` "${CHROOT}/usr/lib"
cp -pRPd /usr/lib/libclam* "${CHROOT}/usr/lib"
cp `ldd "${CHROOT}/usr/lib/libclamav.so" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/lib/libclamav.so" | awk '{print $3}' | grep "^/usr/lib"` "${CHROOT}/usr/lib"
cp `ldd "${CHROOT}/usr/lib/libclamunrar_iface.so" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/lib/libclamunrar_iface.so" | awk '{print $3}' | grep "^/usr/lib"` "${CHROOT}/usr/lib"
cp `ldd "${CHROOT}/usr/lib/libclamunrar.so" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
# Copy user information and necessary libraries for it.
cp -pRPd /lib/libnss* /lib/libnsl* /lib/libresolv* "${CHROOT}/lib"
cp /usr/lib/libnss3.so "${CHROOT}/usr/lib"
grep "^${PN}" "/etc/passwd" > "${CHROOT}/etc/passwd"
grep "^${PN}" "/etc/group" > "${CHROOT}/etc/group"
# fstab stuff.
if [[ `grep "HAVP chroot stuff/ and /usr file system when execute it! ." /etc/fstab` == '' ]]
    then
        cat >> /etc/fstab << EOF
# HAVP chroot stuff.
/var/lib/clamav         ${CHROOT}/var/lib/clamav        none    bind,nodev,noexec,nosuid,rw                                     0 0
/var/log/${PN}          ${CHROOT}/var/log/${PN}         none    bind,nodev,noexec,nosuid,rw                                     0 0
/var/tmp/${PN}          ${CHROOT}/var/tmp               none    bind,nodev,noexec,nosuid,rw                                     0 0
none                    ${CHROOT}/var/run               tmpfs   rw,nodev,noexec,nosuid,relatime,size=1024k,mode=755             0 0
none                    ${CHROOT}/dev                   tmpfs   rw,noexec,nosuid,relatime,size=1024k,nr_inodes=384443,mode=755  0 0
EOF
fi
mount -a
# Configuration.
cp -fpRPd /etc/${PN}/* ${CHROOT}/etc/${PN}/
cd ${WORKD}
cp -f ${PN} /etc/init.d/
cp -f ${PN} ${CHROOT}/etc/init.d/
exit 0
```
### Chrooted Privoxy

**`/etc/init.d/privoxy`**

**chrooted privoxy**

```
#!/sbin/openrc-run
# Copyright 1999-2015 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
CHROOT=/usr/chroot/${SVCNAME}
CONFFILE=/etc/${SVCNAME}/config
PIDFILE=/var/run/${SVCNAME}/${SVCNAME}.pid
depend() {
        need net
}
checkconfig() {
        if [ ! -f "${CHROOT}/${CONFFILE}" ]; then
                eerror "Configuration file ${CHROOT}/${CONFFILE} not found!"
                return 1
        fi
        checkpath --quiet --mode 755 \
                --owner "${SVCNAME}":"${SVCNAME}" \
                --directory `dirname ${CHROOT}${PIDFILE}`
        if [ ! -c ${CHROOT}/dev/null ] ; then
                mknod -m 666 ${CHROOT}/dev/null c 1 3
                mount -ro remount ${CHROOT}/dev
        fi
}
start() {
        checkconfig || return 1
        ebegin "Starting chrooted privoxy"
        start-stop-daemon --start --quiet \
                --pidfile "${CHROOT}${PIDFILE}" \
                --exec chroot ${CHROOT} /usr/sbin/privoxy \
                -- --pidfile "${PIDFILE}" \
                --user privoxy.privoxy "${CONFFILE}" #2>/dev/null
        eend $?
}
stop() {
        ebegin "Stopping chrooted privoxy"
        start-stop-daemon --stop --quiet --pidfile "${CHROOT}${PIDFILE}"
        eend $?
}
```
**`privoxy-chroot.sh`**

**Build chrooted privoxy**

```
#!/bin/bash
# 20150922
# GPL-3
PKGDIR="/var/cache/binpkgs"
CATEGORY="net-proxy"
PN="privoxy"
CHROOT="/usr/chroot/${PN}"
WORKD=`pwd`
# Cleaning chroot directory.
umount "${CHROOT}"/var/log/${PN} "${CHROOT}"/var/run "${CHROOT}"/dev 1>/dev/null 2>&1
rm -rf "${CHROOT}"
# Make common directory and symlinks.
mkdir -p "${CHROOT}"/{dev,etc}
if [ -d /lib64 ]
    then
        mkdir -p "${CHROOT}"/{lib64,usr/lib64}
        cd "${CHROOT}" && ln -s lib64 lib
        cd "${CHROOT}/usr" && ln -s lib64 lib
    else
        mkdir -p "${CHROOT}"/{lib,usr/lib}
fi
# Extract package.
tar -xjphf `ls ${PKGDIR}/${CATEGORY}/${PN}* | tail -n 1` -C "${CHROOT}"
# Copy necessary libraries.
cp -pRPd /lib/ld-* "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/sbin/${PN}" | awk '{print $3}' | grep "^/lib"` "${CHROOT}/lib"
cp `ldd "${CHROOT}/usr/sbin/${PN}" | awk '{print $3}' | grep "^/usr/lib"` "${CHROOT}/usr/lib"
# Copy user information and necessary libraries for it.
cp -pRPd /lib/libnss* /lib/libnsl* /lib/libresolv* "${CHROOT}/lib"
cp /usr/lib/libnss3.so "${CHROOT}/usr/lib"
grep "^${PN}" "/etc/passwd" > "${CHROOT}/etc/passwd"
grep "^${PN}" "/etc/group" > "${CHROOT}/etc/group"
# fstab stuff.
if [[ `grep "Privoxy chroot stuff." /etc/fstab` == '' ]];
    then
        cat >> /etc/fstab << EOF
# Privoxy chroot stuff.
/var/log/${PN}          ${CHROOT}/var/log/${PN}         none    bind,nodev,noexec,nosuid,rw                                     0 0
none                    ${CHROOT}/var/run               tmpfs   rw,nodev,noexec,nosuid,relatime,size=1024k,mode=755             0 0
none                    ${CHROOT}/dev                   tmpfs   rw,noexec,nosuid,relatime,size=1024k,nr_inodes=384443,mode=755  0 0
EOF
fi
mount -a
# Configuration.
cp -frp /etc/${PN}/* ${CHROOT}/etc/${PN}/
cd ${WORKD}
cp -f ${PN} /etc/init.d/
cp -f ${PN} ${CHROOT}/etc/init.d/
exit 0
```
### Chrooted Tor

**`/etc/init.d/tor`**

**Chrooted tor**

```
#!/sbin/openrc-run
# Copyright 1999-2017 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
CHROOT=/opt/torchroot
PIDFILE=/var/run/tor/tor.pid
CONFFILE=/etc/tor/torrc
SVCNAME=tor
GRACEFUL_TIMEOUT=${GRACEFUL_TIMEOUT:-60}
# See bug #523552, and https://trac.torproject.org/projects/tor/ticket/5525
# Graceful = wait 30 secs or so until all connections are properly closed.
extra_commands="checkconfig"
extra_started_commands="graceful gracefulstop reload"
description="Anonymizing overlay network for TCP"
description_checkconfig="Check for valid config file."
description_reload="Reload the configuration."
description_graceful="Gracefully restart."
description_gracefulstop="Gracefully stop."
depend() {
        need net
}
checkconfig() {
        # first check that it exists
        if [ ! -f ${CHROOT}${CONFFILE} ] ; then
                eerror "You need to setup ${CHROOT}${CONFFILE} first"
                eerror "Example is in ${CHROOT}${CONFFILE}.sample"
                return 1
        fi
        if [ ! -c ${CHROOT}/dev/random ] ; then
                mknod -m 666 ${CHROOT}/dev/null c 1 3
                mknod -m 644 ${CHROOT}/dev/random c 1 8
                mknod -m 644 ${CHROOT}/dev/urandom c 1 9
                mount -ro remount ${CHROOT}/dev
        fi
        #checkpath --quiet --mode 755 --owner "${SVCNAME}":"${SVCNAME}" --directory `dirname ${CHROOT}${PIDFILE}`
        # now verify whether the configuration is valid
        /usr/bin/${SVCNAME} --verify-config -f ${CHROOT}${CONFFILE} > /dev/null 2>&1
        if [ $? -eq 0 ] ; then
                einfo "Tor configuration (${CHROOT}${CONFFILE}) is valid."
                return 0
        else
                eerror "Tor configuration (${CHROOT}${CONFFILE}) not valid."
                /usr/bin/${SVCNAME} --verify-config -f ${CHROOT}${CONFFILE}
                return 1
        fi
}
start() {
        checkconfig || return 1
        ebegin "Starting chrooted Tor"
        HOME=/var/lib/${SVCNAME}
        start-stop-daemon --start --pidfile "${CHROOT}${PIDFILE}" --quiet --exec chroot ${CHROOT} /usr/bin/${SVCNAME} -- -f "${CONFFILE}" --runasdaemon 1 --PidFile "${PIDFILE}" > /dev/null 2>&1
        eend $?
}
stop() {
        ebegin "Stopping chrooted Tor"
        start-stop-daemon --stop --pidfile "${CHROOT}${PIDFILE}"
        eend $?
}
graceful() {
        gracefulstop
        start
        eend $?
}
gracefulstop() {
        local rc=0
        ebegin "Gracefully stopping chrooted Tor"
        ebegin "This can take up to ${GRACEFUL_TIMEOUT} seconds"
        start-stop-daemon -P --stop --signal INT -R ${GRACEFUL_TIMEOUT} --pidfile "${CHROOT}${PIDFILE}"
        rc=$?
        eend "done"
        eend $rc
}
reload() {
        if [ ! -f ${CHROOT}${PIDFILE} ]; then
                eerror "${SVCNAME} isn't running"
                return 1
        fi
        checkconfig || return 1
        ebegin "Reloading chrooted Tor configuration"
        start-stop-daemon --signal HUP --pidfile ${CHROOT}${PIDFILE}
        eend $?
}
```
**`tor-chroot.sh`**

**Build chrooted tor**

```
#!/bin/bash
# 20170826
# CC0
# torchroot generation script
export TORCHROOT=/opt/torchroot
mkdir -p $TORCHROOT
mkdir -p $TORCHROOT/etc/tor
mkdir -p $TORCHROOT/dev
mkdir -p $TORCHROOT/usr/bin
mkdir -p $TORCHROOT/usr/lib
mkdir -p $TORCHROOT/usr/share/tor
mkdir -p $TORCHROOT/var/lib
ln -s /usr/lib  $TORCHROOT/lib
# Replace this line if you want to copy your own torrc instead of the default one.
cp /etc/tor/torrc       $TORCHROOT/etc/tor/
cp /usr/bin/tor         $TORCHROOT/usr/bin/
cp /usr/share/tor/geoip* $TORCHROOT/usr/share/tor/
cp /lib/ld-linux-*.so* $TORCHROOT/usr/lib/
cp /lib/libnss* /lib/libnsl* /lib/libresolv* $TORCHROOT/usr/lib/
cp $(ldd /usr/bin/tor | awk '{print $3}' | grep --color=never "^/") $TORCHROOT/usr/lib/
cp -r /var/lib/tor      $TORCHROOT/var/lib/
chown -R tor:tor $TORCHROOT/var/lib/tor
sh -c "grep --color=never ^tor /etc/passwd > $TORCHROOT/etc/passwd"
sh -c "grep --color=never ^tor /etc/group > $TORCHROOT/etc/group"
mknod -m 644 $TORCHROOT/dev/random c 1 8
mknod -m 644 $TORCHROOT/dev/urandom c 1 9
mknod -m 666 $TORCHROOT/dev/null c 1 3
if [[ "$(uname -m)" == "x86_64" ]]; then
  cp /lib64/ld-linux-*.so* $TORCHROOT/usr/lib/
  ln -s /usr/lib ${TORCHROOT}/lib64
fi
```
## Post install tasks

- properly configure iptables, so only tor service can output packets to the internet;
- properly setup proxy variables to all your internet applications and torify them;
- install and properly configure [some privacy addons](https://prism-break.org/en/categories/gnu-linux/#web-browser-addons) to you browser.

## See also

- [Tor](https://wiki.gentoo.org/wiki/Tor) — an onion routing Internet anonymity system.
- [Chroot Guide](https://wiki.gentoo.org/wiki/Project:X86/Chroot_Guide)
- [Chroot](https://wiki.gentoo.org/wiki/Chroot) — a Unix system utility used to change the apparent root directory to create a new environment logically separate from the main system's root directory.
- [Jail](https://wiki.gentoo.org/wiki/Jail) — how to use the **jail** tool to set up a [chroot](https://wiki.gentoo.org/wiki/Chroot)
