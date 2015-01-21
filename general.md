General notes
==========

### Reset all permissions in git repo without reverting changes {{{
Add to git config:
```
git config --global --add alias.permission-reset '!git diff -p -R | grep -E "^(diff|(old|new) mode)" | git apply'
```

Then, run **git permission-reset**
}}}
