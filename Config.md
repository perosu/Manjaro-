# Manjaro初始设置
## 源设置
``` shell
1.pacman-mirrors -i -c China -m rank
```
切换分支

```shell
pacman-mirrors --api --set-branch `分支`
pacman-mirrors --fasttrack 5 && sudo pacman -Syyu
```

返回分支

```shell
pacman -Syyuu
```

合并pacnew

```shell
pacdiff
```

添加archlinuxcn源

``` shell
pacman -S vim
vim /etc/pacman.conf
```
添加
``` shell
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```
刷新数据库
``` shell
pacman -Syy
pacman -S archlinuxcn-keyring
```
为了安全
将SigLevel改成：
``` shell
PackageRequired
```
PS：这个是Blackarch源，对渗透有兴趣的可以添加一下，升级源后需同意一下添加密钥
``` shell
[blackarch]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/blackarch/$repo/os/$arch
```
# 备份包

```shell
pacman -Qqen > pkglist.txt
```

# 备份软件

```shell
pacman -S snapper-gui
snapper -c root create-config /
sanpper -c root undochange `快照版本`..0
#需要btrfs
```
# 设置日志大小

```shell
journalctl --vacuum-size=50M
```

# 防止误删

```shell
pacman -S trash-cli
vim .zshrc
	alias rm=trash-put
```

# 终端Proxy

```shell
alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080"
alias unsetproxy="unset ALL_PROXY"
```

# 安装fish

```shell
pacman -S fish
curl -L https://get.oh-my.fish | fish
chsh -s /usr/bin/fish
```

# Spacevim

```shell
#离线安装
~/.SpaceVim/
~/.cache/vimfiles/
~/.SpaceVim.d/
~/.cache/SpaceVim/
#在线安装
curl -sLf https://spacevim.org/cn/install.sh | bash
```

# 安装zsh

```shell
pacman -S zsh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
chsh -s /usr/bin/zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
vim ~/.zshrc
	ZSH_THEME="agnoster"
:wq
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting
vim ~/.zshrc
	plugins=(
  		git
  		zsh-syntax-highlighting
  		zsh-autosuggestions
	)
:wq
```

# AUR Helper

```shell
pacman -S yay
```

# WPS

```shell
sudo pacman -S wps-office ttf-wps-fonts
```

# WPS中文

```shell
pacman -S wps-office-mui-zh-cn
```

# Deepin Wine Tim

```shell
#IP v6无法解析，禁用IP v6
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1
```

# 中文字体

```shell
sudo pacman -S ttf-roboto noto-fonts ttf-dejavu
# 文泉驿
sudo pacman -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei
# 思源字体
sudo pacman -S noto-fonts-cjk adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts
```

# 小插件

```shell
sudo pacman -S aria2
sudo pacman -S you-get
sudo pacman -S thefuck
sudo pacman -S goldendict
sudo pacman -S visual-studio-code-bin
sudo pacman -S proxychains-ng #终端proxy
```

# 安装latex

```shell
# 安装底层（最后一个用于解决bibtex的问题）
sudo pacman -S texlive-core texlive-langchinese texlive-latexextra texlive-publishers

# 安装IDE
sudo pacman -S texstudio

# 更新texlive
texhash
```

# Home目录映射

```shell
vim ~/.config/user-dirs.dirs
```

# JDK

```shell
yay jdk
archlinux-java status
sudo archlinux-java set java-12-jdk
```



## Linux报警声

``` shell
vim /etc/modprobe.d/blacklist.conf
blacklist pcspkr
```
## 双系统时间不同步
``` shell
timedatectl set-local-rtc 1 --adjust-system-clock
```
```shell
timedatectl set-ntp ture #使用ntp同步
reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1 #Win10
```

## 双显卡冲突

i3wm的双显卡冲突体现在无法关机
``` shell
cd /etc/modprobe.d
vim nvidia-graphics-drivers.conf
```
写入：
``` shell
blacklist nouveau
```
# 屏蔽摄像头

```shell
blacklist uvcvideo
```

## fcitx输入法:

``` shell
pacman -S fcitx fcitx-im fcitx-googlepinyin fcitx-configtool fcitx-skin-material
vim .xprofile（启动图形界面的用户home的根目录）
```
写入：
``` shell
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
使用独显启动程序

``` shell
primusrun ×××
```
# fcitx安装搜狗输入法

```shell
pacman -S fcitx-configtool fcitx-lilydjwg-git fcitx-sgoupinyin
```

# fcitx5

```shell
pacman -S fcitx5-chinese-addons fcitx5 fcitx5-gtk fcitx5-qt fcitx5-pinyin-zhwiki kcm-fcitx5
#fcitx5-pinyinzhwiki 肥猫词库
#fcitx5-pinyin-moegirl 萌娘百科
```

环境变量

```shell
GTK_IM_MODULE=fcitx5
QT_IM_MODULE=fcitx5
XMODIFIERS=”@im=fcitx“
```

主题

```shell
 # ~/.config/fcitx5/conf/classicui.conf
 # 横向候选列表
Vertical Candidate List=False

# 禁止字体随着DPI缩放，避免界面太大
PerScreenDPI=False

# 字体和大小，可以用 fc-list 命令来查看使用
Font="Noto Sans Mono 13"

# 默认蓝色主题
Theme=Material-Color-Blue
```

# Gnome模板

```shell
cd ~/.Template
```

# KDE模板

```shell
cd ~/模板
vim Sample.desktop
	[Desktop Entry]
	Encoding=UTF-8
	Name=Sample
	Comment[zh_CN]=Sample
	Type=Link
	URL=.source/Sample.sample
	Icon=/usr/share/icons/sample.png
:wq
mkdir .source
cd .source
touch Sample.sample
```



# i3wm

## i3wm快捷键
i3wm有一个$mod键，可以定义为win键或者Alt键：
终端
``` shell
$mod Enter
```
左
``` shell
$mod+j
```
上
``` shell
$mod+k
```
下
``` shell
$mod+l
```
右
``` shell
$mod+:
```
退出
``` shell
$mod Shift q
```
stack模式
``` shell
$mod+s
```
tab模式
``` shell
$mod+w
```
tilling模式
``` shell
$mod+e
```
# i3theme

```shell
git clone https://github.com/unix121/i3wm-themer
cd i3wm-themer/
sudo pip install -r requirements.txt
```

## Conky字体问题（i3wm的问题）

i3wm的fcitx的开机启动：
``` shell
vim .i3/config
```
写入：
``` shell
exec --no-startup-id fcitx
```
#wine的桌面有wine程序托盘的问题
tray标准不同
``` shell
sudo pacman -S libappindicator-gtk3
```
安装一个gnome-shell的扩展topicon
#桌面壁纸切换
``` shell
variety
```
## Conky字体问题
/usr/share/conky/conky_maia文件的 conky.text 前4行关于time的，改字体，把bitstream Vera 都改成anti

# envy13 x360开机问题

```shell
cd /etc/default
vim grub
	GRUB_CMDLINE_LINUX="acpi_backlight=native"
:wq
```

# KDE终端隐藏标题栏

```shell
# ~/.config/konsolerc
[KonsoleWindow]
ShowMenuBarByDefault=false
```

