# 通过源码安装Python3.7.3

## 卸载旧的python版本

查看python版本：

```bash
python3 -V
Python 3.6.7
```

卸载旧的python版本：

```bash
# 卸载Python3.6软件包，并删除配置文件
sudo apt-get --purge remove python3.4
```

## 下载python3.7.3源码：

```bash
https://www.python.org/downloads/
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz
# 或者
curl -O https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz
```

解压：

```bash
tar -xzvf Python-3.7.3.tgz
```

## 安装开发者工具包

```bash
# ubuntu:
sudo apt-get install build-essential
# cent os:
sudo sudo yum groupinstall "Development tools"
```

## 安装依赖

```bash
# ubuntu :
# 更新安装源（Source）
sudo apt-get update
# 同时安装多个软件包（已安装的会自动忽略）, -y 表示对所有询问都回答 Yes
sudo apt-get install -y gcc make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \
libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev
#cent os :
sudo yum -y install zlib zlib-devel openssl-devel bzip2-devel expat-devel \
gdbm-devel sqlite-devel libffi-devel
```

## 编译，安装python3.7

```bash
cd Python-3.7.3
sudo ./configure
sudo make
make check # 检查编译是否成功（可选）
sudo make install
```

安装完成后，Python 3.7 安装在了/usr/local文件夹中，可运行文件/usr/local/bin，库文件/usr/local/lib。因为 /usr/local/bin 在 Shell 路径中，所以可以直接在 Shell 中输入如下命令 python3 运行 Python 3.7 解释器

## 创建软链接，方便使用（可选）

```bash
ln -sv /usr/local/python3.7/bin/python3 /usr/bin/python3
ln -sv /usr/local/python3.7/bin/pip3 /usr/bin/pip3
```

在 Shell 中输入 python 运行 python 3.7 解释器

## 更换pip源

临时使用

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

设为默认,升级 pip 到最新的版本 (>=10.0.0) 后进行配置

```bash
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

更换root的pip源

```bash
sudo pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 或者

安装pqi (感谢yhangf:<https://github.com/yhangf>)

```bash
pip install pqi
# 或者
git clone https://github.com/yhangf/PyQuickInstall.git
python3 setup.py install
```

执行pqi

```bash
pqi ls # 列举所有支持的pip源
pqi use <name> # 改变pip源
pqi show # 显示当前pip源
pqi add ustc https://mirrors.ustc.edu.cn/pypi/web/simple # 添加新的pip源(如添加USTC源
pqi remove pypi # 移除pip源（如官方PyPi源）
```

### pip安装模块时可能遇到的问题

1、使用命令：sudo pip install some-package时，可能遇到：
The directory '/home/venus/.cache/pip/http' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/home/venus/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
pip目录和pip/http的属主不是sudo的root用户。如果必须用sudo pip，更改pip目录属主即可：

```bash
sudo chown root /home/venus/.cache/pip/http
sudo chown root /home/venus/.cache/pip
```

如果目录不存在：

```bash
mkdir -p /home/venus/.cache/pip/http
```

2、pip安装时，可能遇到：
/Library/Python/2.7/site-packages/pip/_vendor/requests/packages/urllib3/util/ssl_.py:90: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. For more information, see <https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.>
解决方法：安装requests，注意后面带[security]：

```bash
pip install requests[security]
```