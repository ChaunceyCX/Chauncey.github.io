# 一些额外配置

## vscode

1. 终端zsh乱码:

- 安装powerline字体

```terminal
//没有truetype文件夹就创建一个
$cd /usr/share/fonts/truetype/
$sudo git clone https://github.com/abertsch/Menlo-for-Powerline.git
$sudo fc-cache -f -v
```

- 设置终端字体:

```json
"terminal.integrated.fontFamily": "Menlo for Powerline"
```

2. vscode启动时因为内存溢出导致系统死机
    >设置:

```json
"search.followSymlinks": false
```

