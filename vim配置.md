# vim配置

## 使用vim-plug管理vim插件,安装在/home/venus/.vim

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

## 重命名vimrc.txt为.vimrc于/home/venus/

```bash
sudo vim /home/venus/.vimrc
```

## 复制.vimrc和.vim/至/root/

```bash
sudo cp /home/venus/.vimrc  /root/
sudo cp -r /home/venus/.vim/  /root/
```
