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
cp /home/matt/dotfiles/OpenBSD/sudoers /etc/sudoers
ci -u /etc/sudoers
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
sudo echo '# $Log$
sshguard_flags="-l /var/log/authlog -b 50:/etc/sshban"' >> /etc/rc.conf.local
sudo ci -u /etc/rc.conf.local

sudo ci -u /etc/pf.conf
sudo co -l /etc/pf.conf
sudo echo \
'# $Log$

ext_if = "vio0"
set optimization normal
set skip on lo
table <sshguard> persist

block in quick on $ext_if proto tcp from <sshguard> to any port ssh \
						label "sshguard block"

block in all
pass out all

pass in on $ext_if proto tcp from any to any port ssh'

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
