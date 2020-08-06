# vim配置


## 使用vim-plug管理vim插件,安装在~/.vim/autoload/

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

若网络无法连接
```bash
git clone https://github.com/junegunn/vim-plug
mkdir -p ~/.vim/autoload
cp vim-plug/plug.vim ~/.vim/autoload
```

## 重命名vimrc.txt为.vimrc于~/

```bash
git clone https://github.com/liqiming-whu/memorandum
cd memorandum
cp vimrc.txt ~/.vimrc
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
sudo apt install nodejs yarn
sudo pip install flake8
```

## 复制主题配色文件到colors文件夹下

```bash
cp ~/.vim/plugged/onedark.vim/colors/onedark.vim ~/.vim/colors
cp ~/.vim/plugged/onedark.vim/autoload/onedark.vim ~/.vim/autoload
```
## 复制.vimrc和.vim至/root/

```bash
sudo cp ~/.vimrc  /root/
sudo cp -r ~/.vim  /root/
```
