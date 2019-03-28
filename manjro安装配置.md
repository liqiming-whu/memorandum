# 安装 manjaro : 基于Archlinux的发行版

下载manjaro.iso镜像，推荐使用rufus以dd模式写入u盘启动
[rufus下载地址](https://github.com/pbatard/rufus/releases/download/v3.4/rufus-3.4p.exe)

安装遇到的问题：a start job is running for...

nivida+intel双显卡笔记本--禁用独显

9系显卡：进入安装界面，找到boot，按e进入编辑模式，在 driver=free 后面添加 acpi_osi=! acpi_osi="Windows2009" 按Ctrl+X或F10进入boot，进行安装。

如果这样还是不行的话，重启一下。 将driver=free 改成driver=intel，drive=intel 后面添加 xdriver=mesa acpi_osi=! acpi_osi=”Windows 2009″ 按下F10 启动。

进入系统后，进入命令行 sudo vi etc/default/grub ，修改：GRUB_CMDLINE_LINUX_DEFAULT="splash quiet"为：GRUB_CMDLINE_LINUX_DEFAULT="splash quiet xdriver=mesa acpi_osi=! acpi_osi='Windows 2009'"

10系显卡：暂无解决办法。

## 安装后配置

生成可用中国镜像站列表，会弹出选项框：

```bash
sudo pacman-mirrors -i -c China -m rank
```

刷新缓存，两个 y 代表强制刷新，即使已经是最新的：

```bash
sudo pacman -Syy
```

### pacman 常用命令

```bash
pacman -Sc #清空并且下载新数据
pacman-mirrors -gb testing -c China #更新源
or
pacman-mirrors -c China -g #更新源
pacman -Syu #更新
pacman -Syy #更新源数据库
pacman -Syyu #安装更新
```

## 卸载软件

删除指定软件包，及所有没有被其他已安装软件包使用的依赖关系（s），及配置文件（n），仅供参考：

```bash
sudo pacman -Rsn steam-manjaro ms-office-online hplip firefox manjaro-settings-manager-knotifier octopi-notifier-frameworks manjaro-hello manjaro-documentation-en manjaro-browser-settings yakuake konversation thunderbird kget vlc kdeconnect skanlite
```

VLC用MPV代替

## 更新系统

```bash
sudo pacman -Syu # 同步源(y), 并更新系统(u)
```

更新完，可能会生成 *.pacnew 的新配置文件，必须手动覆盖旧的，可以用 pacdiff 工具，搜索所有电脑中 *.pacnew：

```bash
sudo pacdiff
```

## 安装vim

安装：

```bash
sudo pacman -S vim
```

把.vimrc 放入/home/venus

下载 vim 插件管理器：

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

把.vimrc 和.vim/复制进/root

```bash
sudo cp /home/venus/.vimrc /root/
sudo cp - r /home/venus/.vim/ /root/
```

## 添加 Archlinuxcn 源

Arch Linux 中文社区仓库是由 Arch Linux 中文社区驱动的非官方用户仓库。包含中文用户常用软件、工具、字体/美化包等。

```bash
$ sudo vim /etc/pacman.conf    # 打开文件
# 在文件末尾添加以下两行
[archlinuxcn]
//SigLevel = Optional TrustAll
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

## 安装 archlinuxcn 签名钥匙 (导入 GPG key，否则的话 key 验证失败会导致无法安装软件)

```bash
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

## 使用aur软件

Arch用户软件仓库（Arch User Repository，AUR）是为用户而建、由用户主导的Arch软件仓库。AUR中的软件包以软件包生成脚本（PKGBUILD）的形式提供，用户自己通过makepkg生成包，再由pacman安装。创建AUR的初衷是方便用户维护和分享新软件包，并由官方定期从中挑选软件包进入community仓库。可以使用aur工具完成软件包安装，如yay，aurman。

```bash
sudo pacman -S yay
```

### yay常用命令

```bash
yay {{package_name|search_term}}  #从repos和AUR交互式搜索和安装包
yay | yay -Syu  #同步和更新repos和AUR中的所有包
yay -Sua  #仅同步和更新AUR包
yay -S {{package_name}}  #从repos和AUR安装新包
yay -Ss {{keyword}}  #在包数据库中搜索repos和AUR中的关键字
yay -Ps  #显示已安装包和系统运行状况的统计信息
```

## 安装中文输入法

安装fcitx输入法框架

```bash
yay -S fcitx-im  # 安装fcitx 选择全部安装
yay -S fcitx-configtool  # fcitx 配置界面
```

安装google拼音输入法

```bash
yay -S fcitx-googlepinyin
```

或者安装搜狗拼音输入法

```bash
yay -S fcitx-sogoupinyin
```

配置环境，中文输入 (重启后生效)

```bash
vim ~/.xprofile # 打开编辑.xprofile文件
# 在文件中加入以下两行代码，若文件不存在，则创建该文件
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

## 使用zsh

安装git（manjaro已装）

```bash
yay -S git  #安装
git config --global user.name "*******"
git config --global user.email "******@whu.edu.cn"  #配置个人信息
ssh-keygen -t rsa -C "*******@whu.edu.cn"  #生成ssh密钥
#在 Github 上更新密钥
git config --global core.quotepath false  #做一些设置，让 git status 中的中文不乱码
```

安装zsh并配置

```bash
yay -S zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" # 下载并配置ohmyzsh
chsh -s /bin/zsh  #更换默认bash，重启后生效
```

## 安装中文字体

```bash
yay -S --noconfirm wqy-microhei && fc-cache -fv
```

## 关机卡死

在/boot/grub/grub.cfg里面找到kernel那一行，在最后面加上nouveau.modeset=0,禁用nouveau。

### 安装electron-ssr

```bash
yay -S electron-ssr
```

填写并更新ssr订阅，代理模式PAC

### 安装chrome

```bash
yay -S google-chrome
```

下载chrome拓展switchyomega,改后缀为tar,解压，用chrome开发模式加载安装，配置socket5,127.0.0.1:1080

### 安装vs code

```bash
yay -S visual-studio-code
```

### 安装jdk

```bash
# 配置环境变量
# sudo vim /etc/profile #编辑文件
# 在文件末尾处追加下列几行
export JAVA_HOME=你的jdk解压后的绝对路径
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
```

### 安装Tim,wechat

```bash
yay -S deepin.com.qq.office deepin.com.wechat
# 配置微信，tim 的中文输入问题
# Tim启动脚本位置：/opt/deepinwine/apps/Deepin-TIM/run.sh
# WeChat启动脚本位置：/opt/deepinwine/apps/Deepin-WeChat/run.sh
# 配置tim中文，打开tim启动脚本文件 （微信同理）
sudo vim /opt/deepinwine/apps/Deepin-TIM/run.sh
# 在启动脚本命令之前添加以下内容
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

### 安装终端显示信息 neofetch或screenfetch

```bash
yay -S neofetch
```

### Pyenv

安装：

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

切换到最新稳定版本（git clone下来的是 master 版本，不稳定）

```bash
cd ～/.pyenv
git tag # 检查可用版本
git checkout <tag名> # 切换
```

配置（自动补全功能等）：

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc >> ~/.zshrc
```

下载指定版本 Python

```bash
pyenv install <版本>
```

切换 Python 环境

```bash
pyenv global <版本>
```

### Golang

安装：

```bash
sudo pacman -S go
```

配置，修改 ~/.bashrc 和 ~/.zshrc：

```bash
export GOROOT="/usr/lib/go"
export GOPATH="$HOME/.gopath"
export GOBIN="$GOPATH/bin"
export PATH="$PATH:$GOBIN"
```

### 网络工具包

```bash
sudo pacman -S net-tools  # ifconfig
sudo pacman -S dnsutils  # nslookup
```

### 美化

```bash
sudo pacman -S papirus-icon-theme  #图标
sudo pacman -S ttf-dejavu  #字体
```

### 其他

```bash
yay -S docky  #docky
yay -S netease-cloud-music  #网易云音乐
yay -S pycharm  #pycharm
yay -S nodejs
yay -S python-pip
yay -S typora  # 很好用的Markdown编辑器，支持导出 PDF
yay -S genymotion  # 安卓模拟器
yay -S wps-office ttf-wps-fonts  # WPS和字体
yay -S make
yay -S clang
yay -S electronic-wechat-git #微信
yay -S -S gdb # 默认没有
```

### WiFi问题（解决方法不一定有效）

问题1.知道wifi密码，驱动也有，可以点击连接，总是提示"连接断开，您现在处于离线状态"。

1).打开终端“ctrl+alt+T”

2).输入：

```bash
sudo vim /etc/modprobe.d/iwlwifi.conf
```

3).在文件末尾添加

```bash
options iwlwifi 11n_disable=1
```

4).保存，重启。
ubuntu默认无线连接模式为11n，如果路由未设置，则连接不上。

问题2.经常断网，重启就好

将

```bash
/etc/ppp/options
```

文件中的

```bash
lcp-echo-failure 4
```

改为

```bash
lcp-echo-failure 40
```

或执行命令

```bash
sudo /etc/init.d/networking restart
```
