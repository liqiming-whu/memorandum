# vim配置

## 使用vim-plug管理vim插件,安装在~/.vim/autoload/

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

若网络无法连接

```bash
git clone https://github.com/junegunn/vim-plug
# 或
git clone https://gitee.com/chenxinliang123/vim-plug
mkdir -p ~/.vim/autoload
cp vim-plug/plug.vim ~/.vim/autoload
```

## 重命名vimrc.txt为.vimrc于~/

```bash
git clone https://github.com/liqiming-whu/memorandum
cp memorandum/vimrc.txt ~/.vimrc
```

## 安装插件

```bash
# vim:
:PlugStatus # 查看插件信息
:PlugInstall # 安装插件，首先在.vimrc中声明
:PlugClean # 卸载插件，首先在.vimrc中删除
:PlugUpgrade # 升级
```

## 安装依赖

```bash
sudo apt install universal-ctags nodejs clangd
sudo pip install flake8

```

## 安装yarn

```bash
# archlinux:
sudo pacman -S yarn
# ubuntu:
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
# 如果报错，执行以下命令后重新安装
sudo apt remove cmdtest
sudo apt remove gpg
sudo apt install gnupg1
```

## 安装主题配色

```bash
mkdir ~/.vim/colors
cp ~/.vim/plugged/onedark.vim/colors/onedark.vim ~/.vim/colors
cp ~/.vim/plugged/onedark.vim/autoload/onedark.vim ~/.vim/autoload
```

## 复制.vimrc,.vim,.config至/root

```bash
sudo cp ~/.vimrc  /root
sudo cp -r ~/.vim  /root
sudo cp -r ~/.config  /root
```
