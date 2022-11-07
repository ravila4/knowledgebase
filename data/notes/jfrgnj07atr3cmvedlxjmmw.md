
## List installed repositories

```bash
yum repolist
yum repolist disabled  # disabled repos
```

## Disable a repository


```bash
dnf config-manager --set-disabled repository
```
