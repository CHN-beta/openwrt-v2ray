# openwrt-v2ray

V2Ray for OpenWrt

OpenWrt/LEDE 上可用的 V2Ray

基于 kuoruan 大佬的改了一下，增加了一个选项（删除其它对我来说不必要的东西），以及编译完后用 upx 自动压缩一下。

目前的情况：使用 dokodemo 做透明代理，路由器与服务器的通讯用 ws+tls，不包含 v2ctl 和 geoip，不支持 json，可以压缩到 3.9 M。

## Install

我这里不存在已经编译好的，你需要自己编译，或者去原作者那里，他那里有（没用 upx 压缩过，可以自己解压后压缩再打包回去，当然如果你外接 U 盘的话那就无所谓了）。要是不会搞我也可以帮忙。当然你至少得会看自己路由器的型号，知道服务端怎么部署。这些都不会的话，别问我，我不管。

安装的话：

```
opkg install xxxxx.ipk
```

当然你还得自己写 init.d 啥的，自己查资料吧。

## Custom build

1. Use the **latest** [OpenWrt SDK](https://downloads.openwrt.org/snapshots/) or with source code in master branch (requires golang modules support, commit [openwrt/packages@7dc1f3e](https://github.com/openwrt/packages/commit/7dc1f3e0293588ebc544e8eee104043dd0dacaf5) and later).

**lastest** 就是说，必须要用 snapshot 而不是最新的稳定版本。不过我试了下，稳定版本的 SDK 改一下 feeds，也完全可以。

2. Enter root directory of SDK, then download the Makefile:

```sh
git clone https://github.com/kuoruan/openwrt-v2ray.git package/v2ray-core
```

> For Chinese users, ```export GOPROXY=https://goproxy.io``` before build.

Start build:

```sh
./scripts/feeds update -a
./scripts/feeds install -a

make menuconfig

Languages ---> Go ---> <M> golang-v2ray-core-dev # Source
Network ---> Project V ---> <M> v2ray-core

make package/v2ray-core/clean,compile -j 8 V=s
```

- You can custom the features in "V2Ray Configuration" option.

## Uninstall

```sh
opkg remove v2ray-core
```
