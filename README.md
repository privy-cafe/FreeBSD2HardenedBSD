# FreeBSD2HardenedBSD
##FreeBSD to hardenedBSD up to date conversion
**install unbound and ca_root_nss**
- [ ] pkg install unbound ca_root_nss

**download hbsd-update**
``` -  wget https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/usr.sbin/hbsd-update/hbsd-update
or 
- fetch https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/usr.sbin/hbsd-update/hbsd-update

**Download hbsd-update configuration file**
- wget https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/usr.sbin/hbsd-update/hbsd-update.conf
or
- fetch https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/usr.sbin/hbsd-update/hbsd-update.conf
```
**move to tmp or /opt directory depending on your preferences. **
```
-  cd /tmp 
or
-  cd /opt 

- sed -i -e '/^UNBOUND_HOST=/ s/\/usr\/sbin\/unbound-host/\/usr\/local\/sbin\/unbound-host/' hbsd-update

- wget https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/share/keys/hbsd-update/trusted/dnssec.key-2016-07-30
or
-  fetch https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/share/keys/hbsd-update/trusted/dnssec.key-2016-07-30

- wget https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/share/keys/hbsd-update/trusted/ca.hardenedbsd.org
or
-  fetch https://raw.githubusercontent.com/HardenedBSD/hardenedBSD/hardened/current/master/share/keys/hbsd-update/trusted/ca.hardenedbsd.org
```
**sed the appropriated configuration into unbound,configs and for the upcoming upgrade**
- sed -i -e '/^UNBOUND_HOST=/ s/\/usr\/sbin\/unbound-host/\/usr\/local\/sbin\/unbound-host/' hbsd-update

**make sure that you are sudoers or as root ( by default fbsd does not ship with sudo**
```
sudo -i

 -     mv hbsd-update /usr/sbin
 -     mv hbsd-update.conf /etc
 -     mkdir -p /usr/share/keys/hbsd-update/trusted
 -     mv ca.hardenedbsd.org /usr/share/keys/hbsd-update/trusted
 -     mv dnssec.key-2016-07-30 /usr/share/keys/hbsd-update/trusted
 -     ln -s /usr/share/keys/hbsd-update/trusted/ca.hardenedbsd.org /usr/share/keys/hbsd-update/trusted/5905e1b4.0
 -     ln -s /usr/share/keys/hbsd-update/trusted/dnssec.key-2016-07-30 /usr/share/keys/hbsd-update/trusted/dnssec.key  ```
   **Deploying HardenedBSD**

You may have already guessed: we will use hbsd-update to turn FreeBSD into HardenedBSD and reboot once that’s done.

 -      hbsd-update && reboot

NB: If you’ve checked out the FreeBSD source code to /usr/src, append the -s flag to have hbsd-update extract the HardenedBSD sources in-place. Do make sure to remove corresponding vcs directories like .svn or .git.
```
-       rm -rf /usr/src/{.svn,.git,*}
-       hbsd-update -s && reboot
```
Finally, you’ll notice that many of your installed third-party tools are now perfectly unusable, having been compiled for OpenSSL while we’re using LibreSSL. You’ll start fixing this by reinstalling pkg, and upgrading every installed package.
```
-       pkg-static install pkg
-      pkg upgrade -f
```


**To make full use of HardenedBSD security features, install secadm and secadm-kmod:**
```
  -  git clone https://github.com/hardenedbsd/hardenedbsd-ports ports
  -   make -C ports/hardenedbsd/secadm config install clean
  -  % make -C ports/hardenedbsd/secadm-kmod config install clean
  -  % git clone https://github.com/HardenedBSD/secadm-rules
```
