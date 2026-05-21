![GitHub License](https://img.shields.io/github/license/nikkinikki-org/OpenWrt-momo?style=for-the-badge&logo=github) ![GitHub Tag](https://img.shields.io/github/v/release/nikkinikki-org/OpenWrt-momo?style=for-the-badge&logo=github) ![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/nikkinikki-org/OpenWrt-momo/total?style=for-the-badge&logo=github) ![GitHub Repo stars](https://img.shields.io/github/stars/nikkinikki-org/OpenWrt-momo?style=for-the-badge&logo=github) [![Telegram](https://img.shields.io/badge/Telegram-gray?style=for-the-badge&logo=telegram)](https://t.me/nikkinikki_org)

中文 | [English](README.md)

# Momo

在 OpenWrt 上使用 sing-box 进行透明代理。

## 本 Fork 适配说明（E7G）

本仓库 fork 自 [nikkinikki-org/OpenWrt-momo](https://github.com/nikkinikki-org/OpenWrt-momo)，针对 **OpenWrt 23.05-SNAPSHOT**、**`arm_cortex-a7_neon-vfpv4`**（如 ipq50xx_32 / Nwrt 等）做了额外维护，与上游差异如下：

| 项目 | 上游 | 本 Fork |
|------|------|---------|
| 预编译 ipk | [momomomo.pages.dev](https://momomomo.pages.dev)（无 23.05） | **[GitHub Releases](https://github.com/E7G/OpenWrt-momo/releases)** |
| 一键编译 | 矩阵含 24.10 / 25.12 / SNAPSHOT | 增加 **build-ipq50xx-23.05**（23.05.5 SDK） |
| 适用固件 | 文档建议 ≥ 24.10 | 实测面向 **23.05 + firewall4** 闭源/定制固件 |

### 从 Release 安装（推荐）

1. 打开 [Releases](https://github.com/E7G/OpenWrt-momo/releases)，下载最新 `momo_*.ipk`、`luci-app-momo_*.ipk`（或 `.tar.gz` 解压）。
2. 路由器需已安装 **sing-box**（23.05 官方源多为 1.11.x，可按需自装或 `--force-depends`）。
3. 安装：

```shell
opkg install /tmp/momo_*.ipk /tmp/luci-app-momo_*.ipk
# 依赖冲突时：
opkg install --force-depends /tmp/momo_*.ipk /tmp/luci-app-momo_*.ipk
```

### 自行触发编译并发布 Release

**Actions** → **build-ipq50xx-23.05** → **Run workflow**（可选填 Release 标签）。  
完成后在 **Releases** 下载 ipk，**不再使用** Actions Artifacts / 上游预编译站。

### 订阅 / DNS 提示

使用 clash2sfa 等**全量 sing-box 配置**时，建议 **Core Only** 开启、**Proxy** 关闭；将 DNS 中 `https://223.5.5.5/dns-query` 改为 **UDP `223.5.5.5` 或 `127.0.0.1`**，避免 DoH 超时导致整机卡顿。

上游 Wiki、功能说明仍以 [原项目文档](https://github.com/nikkinikki-org/OpenWrt-momo/wiki) 为准。

## 环境要求

- OpenWrt >= 24.10
- Linux Kernel >= 5.13
- firewall4

## 功能

- 透明代理 (Redirect/TPROXY/TUN, IPv4 和/或 IPv6)
- 访问控制
- 配置文件编辑器
- 定时重启

## 安装和更新

### A. 从软件源安装（推荐）

1. 添加源

```shell
# 只需运行一次
wget -O - https://github.com/nikkinikki-org/OpenWrt-momo/raw/refs/heads/main/feed.sh | ash
```

2. 安装

```shell
# 你可以从 shell 执行命令安装或者从 LuCI 的`软件包`菜单安装
# for opkg
opkg install momo
opkg install luci-app-momo
opkg install luci-i18n-momo-zh-cn
# for apk
apk add momo
apk add luci-app-momo
apk add luci-i18n-momo-zh-cn
```

### B. 从发行版安装

```shell
wget -O - https://github.com/nikkinikki-org/OpenWrt-momo/raw/refs/heads/main/install.sh | ash
```

## 卸载并重置

```shell
wget -O - https://github.com/nikkinikki-org/OpenWrt-momo/raw/refs/heads/main/uninstall.sh | ash
```

## 如何使用

查看 [Wiki](https://github.com/nikkinikki-org/OpenWrt-momo/wiki)

## 如何工作

1. 启动 sing-box。
2. 设置定时重启。
3. 从配置文件读取必要的参数。
4. 配置 IP 规则/路由。
5. 生成防火墙配置并应用。

注意上述步骤可能因配置而变动。

## 编译

### 本地 / SDK

```shell
# 添加源
echo "src-git momo https://github.com/nikkinikki-org/OpenWrt-momo.git;main" >> "feeds.conf.default"
# 更新并安装源
./scripts/feeds update -a
./scripts/feeds install -a
# 编译
make package/luci-app-momo/compile
```

编译结果可以在`bin/packages/your_architecture/momo`内找到。

### GitHub Actions（OpenWrt 23.05）

见上文 [本 Fork 适配说明](#本-fork-适配说明e7g)：运行 **build-ipq50xx-23.05** 后从 **Releases** 获取 ipk。  
亦可使用 **build-packages** 选择性编译：`target_arch=arm_cortex-a7_neon-vfpv4`、`target_branch=openwrt-23.05`（产物仍在 Artifacts，供开发调试）。

## 依赖

- ca-bundle
- curl
- firewall4
- ip-full
- kmod-inet-diag
- kmod-nft-socket
- kmod-nft-tproxy
- kmod-tun
- sing-box >= 1.12

## 贡献者

[![贡献者](https://contrib.rocks/image?repo=nikkinikki-org/OpenWrt-momo)](https://github.com/nikkinikki-org/OpenWrt-momo/graphs/contributors)
