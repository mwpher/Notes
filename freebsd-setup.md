Setup notes for FreeBSD
==========

## Begin/setup sudo {{{

*SU to Root*:
```
pkg install sudo zsh git tmux denyhosts logwatch zfstools bash vim-lite
```
*exit*
```
cd
git clone --recursive https://github.com/mwpher/dotfiles
sh dotfiles/gitsetup.sh
```
*SU*
```
cp /home/matt/dotfiles/easy-sudoers /usr/local/etc/sudoers
ci -u /usr/local/etc/sudoers
```
*Exit*
}}}

## Setup auto-backup [ZFS] {{{

*SU*
```
zfs set com.sun:auto-snapshot=true DATASET
```
Insert into **/etc/crontab**
```
15,30,45 *	*	*	*	root	/usr/local/sbin/zfs-auto-snapshot frequent  4
0        *	*	*	*	root	/usr/local/sbin/zfs-auto-snapshot hourly   24
7        0	*	*	*	root	/usr/local/sbin/zfs-auto-snapshot daily     7
14       0	*	*	7	root	/usr/local/sbin/zfs-auto-snapshot weekly    4
28       0	1	*	*	root	/usr/local/sbin/zfs-auto-snapshot monthly  12
```
}}}

## Setup logging mails {{{

*Add to Crontab*
```
0	8	*	*	*	root	/usr/local/sbin/logwatch.pl
```
*periodic.conf*
```
daily_output="matt"					# user or /file
daily_status_security_output="matt"			# user or /file
weekly_output="matt"					# user or /file
weekly_status_security_output="matt"			# user or /file
monthly_output="matt"					# user or /file
monthly_status_security_output="matt"			# user or /file' >> /etc/periodic.conf
```
}}}

## Specify custom options for ports {{{

```
.if ${.CURDIR:M*/security/sshguard}
  SSHGUARDFW=hosts
.endif
```
}}}

## Setting up Denyhosts {{{

**/etc/rc.conf**
```
# $Id$

denyhosts_enable="YES"
syslogd_flags="-ss -c"

# $Log$
```
```
sudo ci -u /etc/rc.conf
sudo cp /home/matt/dotfiles/denyhosts/denyhosts.conf \
			/usr/local/etc/denyhosts.conf
sudo cp /home/matt/dotfiles/denyhosts/hosts.allow /etc/hosts.allow
sudo touch /etc/hosts.deniedssh
```
**/etc/syslog.conf**
```

# Log denyhosts messages
local7.info					/var/log/wrapper.log
```
```
sudo touch /var/log/wrapper.log
```
**/etc/newsyslog.conf**
```
/var/log/denyhosts 			644  7 	   1024 *     J     /var/run/denyhosts.pid
```
}}}

## Jail notes {{{
*To get ssl, install:*
```
ca_root_nss
```
}}}
