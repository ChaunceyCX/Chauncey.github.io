# manjaro配置以及美化

## 更换国内源
1. 生成中国镜像列表，然后排序，选择最快的镜像，刷新缓存

```
sudo pacman-mirrors -i -c China -m rank
sudo pacman -Syy
```

2. 添加archlinuxcn中文社区仓库

- 添加在“/etc/pacman.conf”文件末尾添加:

```
[srchlinuxcn]
#SidLevel = Optional TrustAll
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

- 安装archlinuxcn-keyring包导入GPGkey

```
sudo pacman -Sy archlinuxcn-keyring
```

## 安装yay

```
sudo pacman -S yay
```

## 对于key错误签名失败之类

```
#移除旧的keys
sudo rm -rf /etc/pacman.d/gnupg

#初始化pacman的keys
sudo pacman-key --init

#加载签名的keys
sudo pacman-key --populates archlinux

#刷新升级已经签名的keys
sudo pacman-key -refresh-keys

#清空并且下载新数据
sudo pacman -Sc

#更新
sudo pacman -Syu
```

## 出现无法锁定db错误

```
sudo rm /var/lib/pacman/db.lck
```

## 安装输入法

```
##遇到搜狗无法启动,删除配置重启之类的需要fcitx-qt4所以安装这个fcitx
yay -S fcitx-lilydjwg-git
yay -S fcitx-qt5
yay -S fcitx-configtool
yay -S fcitx-sogoupinyin
```

创建配置文件~/.xprofile
添加:

```
export GTK_IM_MOUDLE=fcitx
export QT_IM_MOUDLE=fcitx
export XMODIFIERS="@im=fcitx"
```

注销重新登录就可以使用了

## 配置github信息

```
git config --global user.name "ChaunceyCX"
git config --global user.email "chaunceyxie1995@gmail.com"
#存储密码
git config credential.helper store
```

## 安装dockey

啊啊啊!为什么不能安装,gconf-sharp是什么鬼
使用plank替代

## 解决关机30s问题

编辑/etc/systemd/system.conf文件

```

#将

#DefaultTimeoutStopSec=90s

#更改为

DefaultTimeoutStopSec=10s
```

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
mv ./incr-0.2.zsh ~/.oh-my-zsh/plugins/incr/

#autjump
yay -S autojump

```



## 安装中文字体

```
sudo yay -S wqy-microhei wqy-microhei-lite wqy-bitmapfont wqy-zenhei ttf-arphic-ukai ttf-arphic-uming adobe-source-han-sans-cn-fonts
```

## 安装配置npm

```
yay -S npm
#设置淘宝源
npm config set registry https://registry.npm.taobao.org
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


### shell 扩展
 
 - 

## 开机自动挂载硬盘

配置/etc/fstab

```
UUID=0004BF5B00099851                     /home/chauncey/hd1 ntfs nouser,rw 0 0
UUID=096EFA44E563571A                     /home/chauncey/hd2 ntfs nouser,rw 0 0
```

## 终端以及截图快捷键

```
gnome-terminal
flameshot gui
```

## jdk

1. 下载解压并移动到/opt/

```
x ./jdk-8u221-linux-x64.tar.gz
mv xxx /opt/
```

2. 配置环境变量

修改/etc/profile

```
JAVA_HOME=/opt/jdk1.8.0_221
CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

3. 启用配置

```
source /etc/profile
java -version
```

## maven

1. 下载并解压移动到/opt/

```
x ./apache-maven-3.6.1-bin.tar.gz
sudo mv ./apache-maven-3.6.1 /opt/
sudo chown root:root ./apache-maven-3.6.1/
```

2. 配置环境变量

修改/etc/profile

```
MAVEN_HOME=/opt/apache-maven-3.6.1
PATH=${PATH}:${MAVEN_HOME}/bin 
export MAVEN_HOME PATH
```

3. 启用配置

```
source /etc/profile
mvn -v
```

4. 仓库及其它
   
[setting.xml]()

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

## 双系统蓝牙设备连接

1. win10下建立与鼠标的蓝牙配对
2. 切换到manjaro下建立蓝牙鼠标配对
3. 获取配对linkkey

```
sudo -i
cd /var/lib/bluetooth/
ls -l
#进入电脑蓝牙mac目录
cd {PC-mac}
# 找到蓝牙键鼠的mac地址命名的文件夹
cd {mac}
cat info  //查看蓝牙链接信息
复制LinkKey
```

4. 切换到win10,下载[PSTooLs](https://technet.microsoft.com/en-us/sysinternals/bb897553)

将压缩包解压后,以管理员身份运行cmd,在解压目录下执行命令打开特殊注册表

```
PsExec.exe -s -i regedit
```

找到```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys\```

如下图:![regedit](../img/win10_mac_key.png)

5. 讲设备对应的key替换

## vscode zsh乱码

1. 下载需要的字体

```
cd /usr/share/fonts/
sudo git clone https://github.com/abertsch/Menlo-for-Powerline.git
sudo fc-cache -f -v
```

2. 设置vscode中终端的字体

![font](../img/vs_terminal_font.jpg.png)

