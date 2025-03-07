# xunlei
[![CI](https://github.com/gngpp/xunlei/actions/workflows/CI.yml/badge.svg)](https://github.com/gngpp/xunlei/actions/workflows/CI.yml)
<a href="/LICENSE">
    <img src="https://img.shields.io/github/license/gngpp/xunlei?style=flat">
  </a>
  <a href="https://github.com/gngpp/xunlei/releases">
    <img src="https://img.shields.io/github/release/gngpp/xunlei.svg?style=flat">
  </a><a href="hhttps://github.com/gngpp/xunlei/releases">
    <img src="https://img.shields.io/github/downloads/gngpp/xunlei/total?style=flat&?">
  </a>
  [![Docker Image](https://img.shields.io/docker/pulls/gngpp/xunlei.svg)](https://hub.docker.com/r/gngpp/xunlei/)

xunlei从迅雷群晖套件中提取，用于发行版Linux（支持OpenWrt/Alpine/Docker）的迅雷远程下载服务。仅供测试，测试完请自觉删除。

- 支持X86_64/aarch64
- 支持glibc/musl
- 支持更改下载目录
- 支持面板认证
- Docker镜像最小压缩（40MB左右）
- 支持插件：NAS小星（pcdn），测速插件
- 内侧邀请码（3H9F7Y6D/迅雷牛通），内侧码申请快速通道：https://t.cn/A6fhraWZ

```shell
❯ ./xunlei                   
Synology Nas Thunder runs on Linux

Usage: xunlei [OPTIONS] <COMMAND>

Commands:
  install    Install xunlei
  uninstall  Uninstall xunlei
  launcher     Launcher xunlei
  help       Print this message or the help of the given subcommand(s)

Options:
  -d, --debug    Enable debug
  -h, --help     Print help
  -V, --version  Print version

❯ ./xunlei install --help
Install xunlei

Usage: xunlei install [OPTIONS]

Options:
      --debug                          Enable debug
  -U, --auth-user <AUTH_USER>          Xunlei authentication username
  -W, --auth-password <AUTH_PASSWORD>  Xunlei authentication password
  -h, --host <HOST>                    Xunlei Listen host [default: 0.0.0.0]
  -p, --port <PORT>                    Xunlei Listen port [default: 5055]
  -c, --config-path <CONFIG_PATH>      Xunlei config directory [default: /opt/xunlei]
  -d, --download-path <DOWNLOAD_PATH>  Xunlei download directory [default: /opt/xunlei/downloads]
  -m, --mount-bind-download-path <MOUNT_BIND_DOWNLOAD_PATH> Xunlei mount bind download directory [default: /xunlei]
  -h, --help                           Print help

❯ ./xunlei uninstall --help
Uninstall xunlei

Usage: xunlei uninstall [OPTIONS]

Options:
      --debug  Enable debug
  -c, --clear  Clear xunlei default config directory
  -h, --help   Print help

❯ ./xunlei launcher --help 
Launcher xunlei

Usage: xunlei launcher [OPTIONS]

Options:
      --debug                          Enable debug
  -U, --auth-user <AUTH_USER>          Xunlei authentication username
  -W, --auth-password <AUTH_PASSWORD>  Xunlei authentication password
  -h, --host <HOST>                    Xunlei Listen host [default: 0.0.0.0]
  -p, --port <PORT>                    Xunlei Listen port [default: 5055]
  -c, --config-path <CONFIG_PATH>      Xunlei config directory [default: /opt/xunlei]
  -d, --download-path <DOWNLOAD_PATH>  Xunlei download directory [default: /opt/xunlei/downloads]
  -m, --mount-bind-download-path <MOUNT_BIND_DOWNLOAD_PATH> Xunlei mount bind download directory [default: /xunlei]
  -h, --help                           Print help
```

### Ubuntu(Other Linux)
GitHub [Releases](https://github.com/gngpp/xunlei/releases) 中有预编译的 deb包/rpm包，二进制文件，以Ubuntu为例：
```shell
wget https://github.com/gngpp/xunlei/releases/download/v3.8.0-23/xunlei-embed-3.8.0-23-aarch64-unknown-linux-gnu.deb

dpkg -i xunlei_3.8.0-23_amd64.deb

# 安装和运行迅雷程序
xunlei install
# 停止和卸载迅雷程序
xunlei uninstall
# 如果你的系统不支持systemd，则手动启动
xunlei launcher
```

### Docker 运行

```bash
docker run -itd --privileged -p 5055:5055 --hostname=xunlei \
  -v $(pwd)/data:/opt/data \
  -v $(pwd)/downloads:/downloads \
  -e XUNLEI_AUTH_USER=admin \
  -e XUNLEI_AUTH_PASSWORD=admin \
  gngpp/xunlei:latest
```

### OpenWrt 路由器
GitHub [Releases](https://github.com/gngpp/xunlei/releases) 中有预编译的 ipk 文件， 目前提供了 aarch64/x86_64 等架构的版本，下载后使用 opkg 安装，以 nanopi r4s 为例：

```shell
wget https://github.com/gngpp/xunlei/releases/download/v3.8.0-23/xunlei_3.8.0-23_aarch64_generic.ipk
wget https://github.com/gngpp/xunlei/releases/download/v3.8.0-23/luci-app-xunlei_1.0.1-7-1_all.ipk
wget https://github.com/gngpp/xunlei/releases/download/v3.8.0-23/luci-i18n-xunlei-zh-cn_1.0.1-7-1_all.ipk

opkg install xunlei_3.8.0-23_aarch64_generic.ipk
opkg install luci-app-xunlei_1.0.1-7-1_all.ipk
opkg install luci-i18n-xunlei-zh-cn_1.0.1-7-1_all.ipk
```

### 自行编译

```shell
git clone https://github.com/gngpp/xunlei && cd xunlei

# 默认编译在线安装
cargo build --release && mv target/release/xunlei .

# 完整打包编译安装
bash +x ./unpack.sh && cargo build --release --features embed && mv target/release/xunlei .

# 执行安装
./xunlei install
# 若系统不支持systemd，则手动启动daemon
./xunlei launcher
```

### OpenWrt编译

```shell
cd package
svn co https://github.com/gngpp/xunlei/trunk/openwrt
cd -
make menuconfig # choose LUCI->Applications->luci-app-xunlei  
make V=s
```

### FQA
 - 插件依赖bash，系统需要安装bash
 - musl运行库的操作系统，若已存在glibc运行库，那么会优先兼容选择使用操作系统运行库环境（可能会缺依赖，自行补全）
 - 指定运行LD加载库或压缩目前无法做到（二进制带签名），需要逆向打patch
