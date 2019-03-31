# zsh安装与配置

## 安装zsh

```bash
sudo apt install zsh  # 以ubuntu为例
```

或者[通过源码安装]（http://zsh.sourceforge.net/Arc/source.html)

如果没有root权限，通过源码安装，下载解压之后：

```bash
# 首先配置zsh，自定义安装路径
./configure --prefix=$HOME/.local
# 然后编译
make -j4
# 检查编译是否成功（可选）
make check
# 如果没有编译错误，则安装zsh
make install -j4
```

### 将zsh设置为默认shell

如果使用root权限安装的zsh，直接终端运行```chsh -s $(which zsh)```即可。

如果没有root权限，通过源码安装zsh的话，则解决方法是在每次打开终端时执行```exec <zsh-path>```来替代当前的shell。在文件```.bash_profile```中加入：

```bash
[ -f $HOME/.local/bin/zsh ] && exec $HOME/.local/bin/zsh -l
```

如果上述两种方法都不能修改默认shell，安装 oh-my-zsh，安装时会自动切换shell成zsh。

## 安装oh-my-zsh

通过curl

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

或者通过wget

```bash
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

如果上述方法出现问题，可以按照下面的方法进行：

```bash
cd
git clone https://github.com/robbyrussell/oh-my-zsh.git
mv oh-my-zsh .oh-my-zsh
cp .oh-my-zsh/template/zshrc.zsh-template ~/.zshrc
```

## 配置oh-my-zsh

vim .zshrc，切换ZSH_THEME，可以在[这里](<https://github.com/robbyrussell/oh-my-zsh/wiki/Themes>)预览。

想要隐藏用户名，```export DEFAULT_USER="<user-name>"```

### 插件配置

在.zshrc中找到plugins=(git)，其中加入以下插件：

```bash
plugins=(
  git
  extract  # 一个命令 `x` 解压全部压缩文件
  z  # cd的加强版，到达任意到过的位置，模糊匹配
  zsh-syntax-highlighting  # 指令高亮
  zsh-autosuggestions  # 命令自动提示，方向键补全
)
```

其中插件zsh-syntax-highlighting和zsh-autosuggestions需要单独下载，方法如下：

```bash
# 下载zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# 下载zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

如果出现：

```bash
[oh-my-zsh] Insecure completion-dependent directories detected:
drwxrwxrwx 1 venus venus 512 Mar 26 20:22 /home/venus/.oh-my-zsh/custom/plugins/zsh-autosuggestions
drwxrwxrwx 1 venus venus 512 Mar 26 20:22 /home/venus/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting

[oh-my-zsh] For safety, we will not load completions from these directories until
[oh-my-zsh] you fix their permissions and ownership and restart zsh.
[oh-my-zsh] See the above list for directories with group or other writability.

[oh-my-zsh] To fix your permissions you can do so by disabling
[oh-my-zsh] the write permission of "group" and "others" and making sure that the
[oh-my-zsh] owner of these directories is either root or your current user.
[oh-my-zsh] The following command may help:
[oh-my-zsh]     compaudit | xargs chmod g-w,o-w

[oh-my-zsh] If the above didn't help or you want to skip the verification of
[oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
[oh-my-zsh] "true" before oh-my-zsh is sourced in your zshrc file.
```

按照提示，修改权限：

```bash
chmod 755 ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
chmod 755 ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
```

### 使用powerline-theme主题(可选)

#### 下载主题 oh-my-zsh-powerline-theme

```bash
git clone git://github.com/jeremyFreeAgent/oh-my-zsh-powerline-theme 
```

下载完后安装主题，执行目录下的脚本install.sh，此过程只是将主题powerline.zsh-theme放入~/.oh-my-zsh/themes/内

#### 安装主题所需要的字体，否则会乱码

```bash
git clone https://github.com/powerline/fonts.git
sudo ./fonts/install.sh
```

到此字体安装完成，之后在终端命令行工具的偏好设置设置:
找到“文本->>字体->>更改” ，“所有字体”中选中“Meslo LG M for powerLine” 字体

#### 设置oh my zsh 配置文件

```bash
vim  ~/.zshrc   # vim 编辑 zshrc 配置文件
ZSH_THEME="robbyrussel"  # 修改此项为设置主题： ZSH_THEME="powerline"
# 修改此项以更好的支持自己常用命令：plugins=(git autojump osx brew node npm)
```

### 自定义主题

默认的主题robbyrussel不显示用户名，自定义主题可以修改使其显示用户名

终端输入：

```bash
cd ~/.oh-my-zsh/themes
vim robbyrussell.zsh-theme
```

可以看到：

```bash
local ret_status="%(?:%{$fg_bold[green]%}➜ :%{$fg_bold[red]%}➜ )"
PROMPT='${ret_status} %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"
```

PROMPT就是设置显示的用户名
由于oh_my_zsh时常会有版本更新，为了避免我们修改的跟更新的版本有冲突，建议不要修改robbyrussell.zsh-theme，而是将其拷贝出来，命名为自己的主题文件，比如叫做mytheme.zsh-theme，然后只对mytheme-theme进行修改。

```bash
cp robbyrussell.zsh-theme mytheme.zsh-theme
```

修改后将 ~/.zshrc 中的```ZSH_THEME="robbyrussell"```改为```ZSH_THEME="mytheme"```

#### 参考设置

```bash
PROMPT='%{$fg[green]%}%m@%{$fg[magenta]%}%(?..%?%1v)%n:%{$reset_color%}%{$fg[cyan]%}%~#'
```

```bash
PROMPT='%{$fg_bold[red]%}-> %{$fg_bold[green]%}%p%{$fg[cyan]%}%d %{$fg_bold[blue]%}$(git_prompt_info)%{$fg_bold[blue]%}% %{$reset_color%}~#:'
```

```bash
PROMPT='%{$fg_bold[red]%}-> %{$fg_bold[green]%}%p%{$fg[cyan]%}%d %{$fg_bold[blue]%}$(git_prompt_info)%{$fg_bold[blue]%}% %{$fg[magenta]%}%(?..%?%1v)%{$reset_color%}~#: '
```

```bash
PROMPT='%{$fg_bold[red]%}-> %{$fg_bold[magenta]%}%n%{$fg_bold[cyan]%}@%{$fg[green]%}%m %{$fg_bold[green]%}%p%{$fg[cyan]%}%~ %{$fg_bold[blue]%}$(git_prompt_info)%{$fg_bold[blue]%}% %{$fg[magenta]%}%(?..%?%1v)%{$fg_bold[blue]%}? %{$fg[yellow]%}# '
```

```bash
%T 系统时间（时：分)
%* 系统时间（时：分：秒）
%D 系统日期（年-月-日）
%n 你的用户名
%B - %b 开始到结束使用粗体打印
%U - %u 开始到结束使用下划线打印
%d 你目前的工作目录
%~ 你目前的工作目录相对于～的相对路径
%M 计算机的主机名
%m 计算机的主机名（在第一个句号之前截断）
%l 你当前的tty
%n 登录名
```

### mytheme.zsh-theme

修改：

```bash
PROMPT='${ret_status}%{$fg[magenta]%}%n@%{$fg[green]%}%m %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'
```

全文：

```bash
local ret_status="%(?:%{$fg_bold[green]%}➜ :%{$fg_bold[red]%}➜ )"
PROMPT='${ret_status}%{$fg[magenta]%}%n@%{$fg[green]%}%m %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"
```

下载```mytheme.zsh-theme.txt```，重命名```mytheme.zsh-theme```于```~/.oh-my-zsh/themes```，在```~/.zshrc```中声明```ZSH_THEME="mytheme"```,然后```source ~/.zshrc```

```bash
cp mytheme.zsh-theme.txt ~/.oh-my-zsh/themes/mytheme.zsh-theme
vim ~/.zshrc
source ~/.zshrc
```

### 随机主题

在 ```~/.zshrc``` 文件中设置主题为 ```random``` 即可开启随机主题

```bash
ZSH_THEME="random"
```

每次打开新的终端的时候，zsh 都会随机使用一个主题
