General notes
=============

Reset all permissions in git repo without reverting changes
-----------------------------------------------------------

Add to git config:

    git config --global --add alias.permission-reset '!git diff -p -R | grep -E "^(diff|(old|new) mode)" | git apply'

Then, run **git permission-reset**

Bacula SSH Backup
-----------------

See: http://wiki.bacula.org/doku.php?id=sshtunnel

Misc
------

Install **pwgen**.

```sh
# Generate a secure 16-char password
pwgen -s -c -n -B -y 16 1

# Generate 10 to choose from
pwgen -s -c -n -B -y 16 10
```
