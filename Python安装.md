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
sudo apt-get --purge remove python3.6
```

python3.6执行文件和软链接在/usr/bin,库文件在/usr/lib。

## 下载python3.7.3源码：
[*Download Python*](<https://www.python.org/downloads/>)

```bash
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
sudo ./configure  # --prefix=/usr/local/python3.7(默认安装目录) --enable-shared CFLAGS=-fPIC --enable-optimizations
sudo make
make check # 检查编译是否成功（可选）
sudo make install
```

--enable-optimizations 是优化选项(LTO，PGO 等)加上这个 flag 编译后，性能有 10% 左右的优化。
这里加上--enable-shared和-fPIC之后可以将python3的动态链接库编译出来，默认情况编译完lib下面只有python3.xm.a这样的文件，python本身可以正常使用，但是如果编译第三方库需要python接口的比如caffe等，则会报错；所以这里建议按照上面的方式配置。

如果不指定安装目录，Python 3.7 默认安装在了/usr/local/文件夹中，可运行文件/usr/local/bin，库文件/usr/local/lib。因为 /usr/local/bin 在 Shell 路径中，所以可以直接在 Shell 中输入 python3 运行 Python 3.7 解释器

## 将python库的路径写到/etc/ld.so.conf配置中

```bash
cd /etc/ld.so.conf.d
sudo vim python3.7.conf
# 添加python库路径，默认为
/usr/local/lib # 或者/安装目录/lib
# :wq保存退出
sudo ldconfig  #启动配置
```

## 创建软链接，方便使用（可选）

```bash
sudo ln -sv /usr/local/bin/python3.7 /usr/bin/python
sudo ln -sv /usr/local/bin/pip3 /usr/bin/pip
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
sudo -i
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 或者

安装pqi (感谢[*yhangf*](<https://github.com/yhangf>))

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

### 安装代码审查

```python
pip install flake8
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

### 后台运行python程序输出缓冲区问题

打印打印一颗”*”休息1s
code:

```python
#!/usr/bin/python
#coding=utf-8
'''
暂停1s输出
'''
import time

def printStar(n):
        for i in range(n):
                print " * ",
                time.sleep(1)

if __name__ == '__main__':
        printStar(10)
```

输出结果（等待10s后一次性输出）：

```bash
[root@computername test]# python sleep.py 
*   *   *   *   *   *   *   *   *   * 
```

原因：在运行代码时，打印10个"*"没有占满缓存区，所以等到程序结束时，才会一次性输出。

```bash
缓冲区的刷新方式：
    1.flush()刷新缓存区
    2.缓冲区满时，自动刷新
    3.文件关闭或者是程序结束自动刷新
```

解决办法：

1、运行时加-u参数，如 # python -u test.py
用man查看python的-u参数，说明如下：

```bash
Force stdin, stdout and stderr to be totally unbuffered. On systems where it matters, also put stdin, stdout and stderr in binary mode. Note that there is internal buffering in xreadlines(), readlines() and file-object iterators (“for line in sys.stdin”) which is not influenced by this option. To work around this, you will want to use “sys.stdin.readline()” inside a “while 1:” loop.
```

强制stdin，stdout和stderr完全不缓冲。

2、在不要缓冲的每一次print后面添加sys.stdout.flush()函数

3、脚本头部添加-u，如#!/usr/local/bin/python -u，道理同方法1

4、添加环境变量 PYTHONUNBUFFERED=1
