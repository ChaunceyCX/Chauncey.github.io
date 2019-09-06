## 安装输入法

```
##遇到搜狗无法启动,删除配置重启之类的需要fcitx-qt4所以安装这个fcitx
yay -S fcitx-configtool fcitx-sogoupinyin fcitx-qt4
```

创建配置文件~/.xprofile
添加:

```
export GTK_IM_MOUDLE=fcitx
export QT_IM_MOUDLE=fcitx
export XMODIFIERS="@im=fcitx"
```

注销重新登录就可以使用了

## 安装dockey

啊啊啊!为什么不能安装,gconf-sharp是什么鬼
使用plank替代

## zsh安装配置

1. 查看当前系统shell

```
echo $SHELL
/bin/bash
```

2. 查看系统是否安装了zsh

```
cat /etc/shells
/bin/sh
/bin/bash
/bin/zsh
/usr/bin/zsh
/usr/bin/git-shell
```

3. 切换shell 为 zsh(重启生效)

```
chsh -s /bin/zsh
```

4. 安装oh-my-zsh

```
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
```

5. 修改主题等

```
vim ~/.zshrc
```

配置文件备份位置:
[.zshrc](https://github.com/ChaunceyCX/my-config-files/blob/master/zsh/.zshrc)


6. 更新配置

```
source ~/.zshrc
```

7. 安装插件:

```
#自动推荐
git clone git://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions

#高亮
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting

#高亮版cat
yay -S bat

#自动补全
wget http://mimosa-pudica.net/src/incr-0.2.zsh
mkdir /home/chauncey/.oh-my-zsh/plugins/incr/
mv ./incr-0.2.zsh ~/.oh-my-zsh/plugins/incr/

#autjump
yay -S autojump

```

## 美化

```
#安装ocs-url
yay -S ocs-url
```

[gnome look](https://www.gnome-look.org/p/1013030/)

- 应用:Flat-Remix-GTK-Blue-Darker-Solid
- 图标:Papirus
- Shell:Flat-Remix
- GDM(Loc):
  - 1. ocs安装[master](https://github.com/daniruiz/flat-remix-gnome/archive/master.zip)
  - 2. 安装相关依赖:

    yay -S glib2 imagemagick 
  - 3. sudo make install

## 终端以及截图快捷键

```
gnome-terminal
flameshot gui
```

## 蓝牙音响没有声音

1. 安装必要的包

```
yay -S bluez bluez-utils pulseaudio-bluetooth pavucontrol pulseaudio-alsa pulseaudio-bluetooth-a2dp-gdm-fix
```

2. 重启蓝牙服务

```

systemctl restart bluetooth

```

3. 重新启动pulseaudio服务

```
sudo killall pulseaudio
```

4. 移除耳机重新链接


## 程序以及shell 扩展
 
- google-chrome
- vscode
	- zh
	- markdown-all-in-one
	- IntelliJ IDEA Keybindings
- idea
- telegram
- plank
- flameshot
- shell-extension
  - Simple net speed 
  - Coverflow Alt-Tab
  - TopIcons Redux
