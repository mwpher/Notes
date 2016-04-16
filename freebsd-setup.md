Setup notes for FreeBSD
==========

## Begin/setup sudo {{{

*SU to Root*:
```
pkg install sudo zsh git tmux denyhosts logwatch zfstools bash vim-lite pwgen
```
*exit*
```
cd
git clone --recursive https://github.com/mwpher/dotfiles
sh dotfiles/gitsetup.sh
```
*SU*
```
cp /usr/local/etc/sudoers /usr/local/etc/sudoers.tmp
cat /home/matt/dotfiles/sudoers > /usr/local/etc/sudoers.tmp
vi /usr/local/etc/sudoers.tmp # Customize appropriately
mkdir /usr/local/etc/sudoers.d
visudo -csf /usr/local/etc/sudoers.tmp && mv /usr/local/etc/sudoers.tmp /usr/local/etc/sudoers
ci -u /usr/local/etc/sudoers
rm /etc/sudoers.tmp
echo '/var/log/sudo.log 	root:wheel 	600  10	   100	*     JC' > /etc/newsyslog.conf
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

## Jail notes {{{
*To get ssl, install:*
```
ca_root_nss
```
}}}
