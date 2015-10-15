Setup notes for OpenBSD
==========

### Begin/setup sudo {{{
**SU to Root**
**Add screen-clearing on logout:**
Add the following line to the "default" entry in gettytab(5):
```
:cl=\E[H\E[2J:
```
```
pkg_add zsh git bash vim sshguard
```
**Exit**

*Now you run the general.sh script*

**start tmux and SU to Root**

Then, add a *mail alias* for getting root status mails:
```
echo 'root: matt' >> /etc/mail/aliases
newaliases
```
```
cp /etc/sudoers /etc/sudoers.tmp
cat /home/matt/dotfiles/OpenBSD/sudoers > /etc/sudoers.tmp
vi /etc/sudoers.tmp # Customize appropriately
visudo -csf /etc/sudoers.tmp && mv /etc/sudoers.tmp /usr/local/etc/sudoers
ci -u /etc/sudoers
rm /etc/sudoers.tmp
echo '/var/log/sudo.log 	root:wheel 	600  7	   *	168   Z' \ > /etc/newsyslog.conf
```
**Exit**

**Install system patches**
```
sh /home/matt/dotfiles/OpenBSD/openup.sh
```
}}}

### Setup SSHguard {{{
```
sudo ci -u /etc/rc.conf.local
sudo co -l /etc/rc.conf.local
```

```
# $Id$

sshguard_flags="-l /var/log/authlog -b 50:/etc/sshban"

# $Log$
```
```
sudo ci -u /etc/rc.conf.local

sudo ci -u /etc/pf.conf
sudo co -l /etc/pf.conf
```
```
# $Id$

ext_if = "vio0"
set skip on lo
table <sshguard> persist

block in quick on $ext_if proto tcp from <sshguard> to any port ssh \
						label "SSHguard block"

block in all
pass out all

pass in log on $ext_if proto tcp from any to any port ssh label "SSH"'

# $Log$
```


```bash
sudo ci -u /etc/pf.conf
pfctl -vnf /etc/pf.conf
pfctl -f /etc/pf.conf
/etc/rc.d/sshguard start
```
}}}

### Setup logging mails {{{

```
sudo crontab -e
```
```
0	8	*	*	*	/usr/sbin/logwatch
```
}}}
