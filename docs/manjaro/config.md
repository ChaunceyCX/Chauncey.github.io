# fstab开机挂载ntfs

>比如：在 /etc/fstab里面，挂载时可以指定用户id和组id...貌似只支持ntfs, fat32不支持

```
UUID=*** /mount/point1 ntfs defaults,umask=007,uid=1000,gid=1000 0 0
```

## vscode

1. 终端zsh乱码:
   >设置中:

```json
"terminal.integrated.fontFamily": "Menlo for Powerline"
```

2. vscode启动时因为内存溢出导致系统死机
    >设置:

```json
"search.followSymlinks": false
```

