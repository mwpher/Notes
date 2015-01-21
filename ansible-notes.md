ansible notes
==========

### Notes about jails on FreeBSD
**Make a direct connection to the jail**
*hosts file*
```
[jails:children]
www

[www]
www_keltia_net ansible_connection=jail
```

### only run task when variable contains something
```
with_items: databases
when: databases|length > 0
```
