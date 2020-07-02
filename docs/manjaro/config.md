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

## 网易云音乐字体太小

>将 /usr/share/applications/netease-cloud-music.desktop 拷贝到 ~/.local/share/applications/，然后修改其中的 Exec 字段

```
Exec=netease-cloud-music --force-device-scale-factor=1.25 %U
```

## deepin-wine-wechat

> 进入相关容器的配置界面调整：显示->dpi

```
env WINEPREFIX="$HOME/.deepinwine/Deepin-WeChat" winecfg
```
