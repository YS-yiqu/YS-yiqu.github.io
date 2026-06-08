---
title: "告别付费远程桌面：Tailscale+RustDesk 免费方案"
date: 2026-06-08T13:04:00+08:00
lastmod: 2026-06-08T13:04:00+08:00
description: "TeamViewer 弹窗、AnyDesk 延迟、向日葵限速？Tailscale+RustDesk 直连方案，免费不限速不弹窗。"
tags: ["光猫", "远程桌面", "Tailscale", "RustDesk", "P2P"]
categories: ["系列"]
series: ["光猫折腾记"]
series_weight: 4
draft: false
---

## 故事开头

远程桌面的痛苦：

```
TeamViewer → 连接 5 分钟 → 弹窗「请购买商业版」→ 我长得很像企业吗？😭

AnyDesk    → 中转服务器在国外 → 延迟高到：点一下鼠标，咖啡都凉了

向日葵     → 免费版限速 1Mbps → 看幻灯片般的体验
```

我需要的是：**坐在咖啡店，连回家里的电脑，不弹窗、不限速、不花钱。**

答案：Tailscale + RustDesk。

---

## 原理：Tailscale 怎么让远在天边的电脑变成近在咫尺

### 传统远程桌面的问题

```
你的电脑 ──→ 远程桌面软件的中转服务器 ←── 家里电脑
                    │
            所有数据经过这里
            服务器在国外 → 延迟高
            服务器要钱 → 免费版限速限时
```

### Tailscale 的做法：P2P 直连

```
你的电脑 ←─── IPv6 直连 ───→ 家里电脑
        （不经过任何服务器）
```

Tailscale 做的事：

```
Step 1: 每台设备装 Tailscale，用同一账号登录
Step 2: Tailscale 服务器给每台设备发「身份证书」
Step 3: 设备之间用 WireGuard 协议建立加密直连隧道
Step 4: 每台设备获得一个固定的虚拟局域网地址（100.x.x.x）
Step 5: 数据不经过 Tailscale 服务器，直接 P2P 传输
```

用通俗的话说：

> Tailscale 像一个「只认熟人的小区门禁系统」。
> 你拿着门禁卡（私钥），在门口刷卡 → 门禁核对 → 是你 → 放你进来。
> 别人知道你家地址也没用——没门禁卡进不来。
> 你串门不用经过物业——直接走过去就行。

### 为什么用 RustDesk 而不是 Windows 自带 RDP？

Windows 11 **家庭版**没有远程桌面服务端——这是微软的产品策略，专业版才有。RDP Wrapper 可以绕过限制，但最新的 24H2 版本已不兼容（`termsrv.dll` 的补丁偏移量变了）。

RustDesk 是开源的远程桌面工具，免费、不限速、支持 IPv6 直连。装两个软件（Tailscale + RustDesk）就能用。

---

## 操作步骤

### 1. 所有设备装 Tailscale

| 设备 | 下载 |
|------|------|
| Windows | https://tailscale.com/download-windows |
| macOS | App Store 搜 Tailscale |
| iOS/Android | App Store / Google Play |
| Linux / NAS | `curl -fsSL https://tailscale.com/install.sh \| sh` |

装完用同一个账号登录（Google/GitHub/Microsoft 都行）。

### 2. 查看 Tailscale 网络

```bash
tailscale status
```

输出：

```
100.102.38.78   笔记本     windows  ✅
100.64.161.84   NAS        linux    ✅
100.126.229.71  另一台电脑  windows  ✅
```

每台设备有自己固定的 `100.x.x.x` 地址。

### 3. 装 RustDesk + 开 IP 直连

下载：https://github.com/rustdesk/rustdesk/releases

**关键设置**（被控制的电脑上）：

```
RustDesk → 设置 → 安全 → 允许通过IP连接 → 开启
```

设置连接密码。

### 4. 连接

控制端电脑上：

```
RustDesk → 连接方式选「IP/主机名」→ 填 100.102.38.78 → 输入密码 → ✅
```

---

## 速度对比

| 方案 | 延迟 | 限制 |
|------|------|------|
| TeamViewer 免费版 | 高（中转） | 5 分钟弹窗 |
| AnyDesk 免费版 | 中（中转） | 无时间限制 |
| 向日葵免费版 | 低 | 限速 1Mbps |
| **Tailscale + RustDesk** | **极低（P2P）** | **无双限制** |

同城延迟 5-10ms，跨省 20-30ms。和坐在电脑前几乎没有差别。

---

## 安全说明

| 风险 | 说明 |
|------|------|
| Tailscale 认证服务器 | 由 Tailscale 公司运营。他们能看到你的设备列表和公钥，但看不到你的数据和私钥 |
| 数据加密 | WireGuard 端到端加密 |
| 服务器倒闭 | 已连接的设备继续用，新设备无法认证 |

想完全自己掌控？用开源的 Headscale 自建认证服务器。

---

## 进阶：Tailscale 还能做什么

```
远程桌面    → RustDesk + Tailscale
文件共享    → Windows SMB \\100.x.x.x
SSH        → ssh user@100.x.x.x
内网穿透    → 没有公网 IP 也能从外面访问
手机访问 NAS → Files App 填 smb://100.x.x.x
```

---

## 一句话

> 两个开源工具，加起来 50MB。
> 省掉一年几百块的远程桌面会员。
> 还不用忍受弹窗。

---



---

---

## 📝 系列导航

| [← 上一篇](/posts/ont-ipv6/) | [系列总览](/posts/ont-series-index/) | [下一篇 →](/posts/ont-nas-file-share/) |
