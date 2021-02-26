1.加快拉取代码速度、安装软件速度（可选）
---

修改ubuntu源镜像
---

备份/etc/apt/sources.list文件

```bash
mv /etc/apt/sources.list /etc/apt/sourses.list.backup
```
新建/etc/apt/sources.list文件并添加以下内容

```
#阿里云源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```
然后执行
```bash
sudo apt  update
```

安装apt-fast
---
```bash
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get update
sudo apt-get -y install apt-fast
```

2.拉取llvm代码
---

先提高从github拉取代码的速度
在命令行下执行
```bash
nslookup github.com
#输出类似Address: 52.74.223.119
nslookup github.global.ssl.fastly.net
#输出类似Address: 104.16.252.55
sudo gedit /etc/hosts
#在hosts文件末尾添加
#github.com 52.74.223.119
#github.global.ssl.fastly.net 104.16.252.55
sudo /etc/init.d/networking restart
```

拉取llvm-project代码
```bash
git clone https://github.com/llvm/llvm-project
```

3.编译llvm代码
---

更新cmake
```bash
#检查openssl开发库是否安装
sudo apt-fast install openssl
sudo apt-fast install libssl-dev

#下载cmake源代码
wget https://cmake.org/files/v3.19/cmake-3.19.6.tar.gz
tar -xvf cmake-3.19.6.tar.gz
cd cmake-3.19.6
./configure
make -j4
sudo apt remove cmake #删除原版cmake(只针对apt安装有效)
sudo make install
cmake --version #预期输出3.19.6
```

编译llvm代码
```bash
cd llvm-project/llvm
mkdir build
cd build
cmake ..
make -j4
```