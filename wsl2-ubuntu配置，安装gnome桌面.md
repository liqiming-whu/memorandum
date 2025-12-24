# wsl2 ubuntu配置，安装gnome桌面

## 1.安装ubuntu

打开powershell, 输入

```
wsl --install
```

根据提示创建用户并设置密码
更新系统

```
sudo apt update && sudo apt upgrade
sudo apt autoremove
```

## 2.设置中文

```
sudo apt install language-pack-zh-hans
sudo dpkg-reconfigure locales
```

中途会见到一个TUI，选择zh_CN.UTF8 UTF8即可
重启终端生效

安装字体

```
sudo apt install fontconfig # 可能默认已安装
sudo apt install fonts-noto-cjk fonts-noto-cjk-extra
```

复制windows字体

```
sudo cp -r /mnt/c/Windows/Fonts /usr/share/fonts/windows
fc-cache -f -v
```

## 3.安装 GNOME 图形界面

安装ubuntu-desktop gnome

```
sudo apt install ubuntu-desktop gnome
```

## 4.安装 xrdp（远程桌面协议）

```
sudo apt install xrdp
```

## 5.编辑xrdp配置文件

```
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak # 备份
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini # 更改端口为3390
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini # 颜色质量改为128位
sudo sed -i 's/#xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
```

## 6.查看/编辑启动文件 startwm.sh

```
sudo vim /etc/xrdp/startwm.sh
```

注释或删除下面几行

```
if test -r /etc/profile; then
        . /etc/profile
fi

if test -r ~/.profile; then
        . ~/.profile
fi

test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```

添加如下内容

```
# 清理可能影响会话的环境变量
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR

# 设置 GNOME 环境变量
export GNOME_SHELL_SESSION_MODE=ubuntu
export XDG_CURRENT_DESKTOP=ubuntu:GNOME
export XDG_DATA_DIRS=/usr/share/ubuntu:/usr/local/share:/usr/share:/var/lib/snapd/desktop
export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg

# 直接启动 GNOME 会话（跳过 /etc/X11/Xsession）
exec /usr/bin/gnome-session
```

## 7.启动服务

```
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

## 连接rdp

打开远程桌面连接输入
```
localhost:3390
```