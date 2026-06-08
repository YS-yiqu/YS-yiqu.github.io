---
title: "AI 跑完任务自动微信通知：WxPusher 零依赖方案"
date: 2026-05-21T10:00:00+08:00
lastmod: 2026-06-08T18:00:00+08:00
description: 'AI 跑训练、编译、下载要等好久？用 WxPusher 加上微信通知，泡面好了自动弹消息。'
tags: ["自动化", "AI", "工具技巧"]
categories: ["技术"]
draft: false
---

> 一个标准库就能搞定的自动通知方案，AI 帮你跑完耗时任务，微信自动弹消息。

## 背景

用 AI coding assistant 干活越来越多了。跑个训练、编译个东西、下个大文件，动不动几分钟甚至更久。要是能跟泡面一样——"叮！好了！"——就不用隔一会儿去窗口看一眼了。

## 原理

```
Python 脚本 ──POST JSON──> WxPusher API ──推送──> 微信服务号 "WxPusher 推送服务"
```

WxPusher 是一个开源的消息推送平台，核心价值就一个：**通过微信服务号免费推送消息，不限条数**。

整个链路很简单：一段不超过 30 行的 Python 脚本，用标准库（`urllib` + `json`）构造 POST 请求，发给 WxPusher 的 API。WxPusher 收到后转发到微信服务号。不需要安装任何第三方包。

## 配置步骤（一次配置，永久可用）

1. 打开 [wxpusher.zjiecode.com](https://wxpusher.zjiecode.com)，微信扫码登录
2. 创建应用 → 拿到 AppToken（`AT_` 开头）
3. 扫描应用关注二维码 → 拿到 UID（`UID_` 开头）
4. 把两个密钥填入 `notify.py`

> ⚠️ 一定要在微信里关注「WxPusher 推送服务」公众号，否则收不到消息。消息不是弹在聊天列表，而是在公众号里面。

## 接入代码

核心就一个 `send()` 函数，十几行搞定：

```python
def send(title, content=""):
    body = json.dumps({
        "appToken": APP_TOKEN,
        "content": f"{title}\n\n{content}",
        "contentType": 1,
        "uids": UIDS
    }).encode("utf-8")
    req = urllib.request.Request(API_URL, data=body, headers=HEADERS)
    with urllib.request.urlopen(req) as resp:
        return resp.read().decode("utf-8")
```

零依赖，标准库即可，Python 3 任一版本都能跑。

## 自动通知：让 AI 帮你加

仅仅是脚本还不够——你不想每次都自己敲 `&& notify.py`。解决思路是**让 AI assistant 记住规则**：只要检测到耗时命令，自动在后面接上通知。

在 AI assistant 的高优先级记忆中写入：

```
每次运行超过30秒的命令 → 自动接上 notify.py
```

之后你只要告诉 AI "帮我跑训练"，它自动输出：

```bash
python train.py && py notify.py "训练完成"
```

你不需要多想一步。

## 经验教训

- **不要自己做系统级进程监视器**。一开始走了弯路：写了个后台 Python 进程，监听 `wmic process` 中超过 30 秒的进程，退出后自动通知。这条路行不通——根本分不清哪个是你手动的、哪个是后台系统进程，结果手机被弹爆了 😅。正确思路是**让 AI assistant 做这件事**，它天然知道现在跑的是什么命令、预估要多久。
- **零依赖很重要**。用标准库就能解决的，不要引入第三方包——换台机器也能跑。
- **公众号消息 vs 聊天弹窗**。微信服务号的推送机制跟好友消息不一样，必须要先关注公众号。不少人配置完收不到消息就卡在这一步。
