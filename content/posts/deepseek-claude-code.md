---
title: "用 DeepSeek 跑 Claude Code"
date: 2026-05-24T10:00:00+08:00
lastmod: 2026-06-08T18:00:00+08:00
description: 'Claude Code 连 DeepSeek 要搭代理？别急，DeepSeek 早就原生支持 Anthropic 协议了。三个坑踩完，一条命令直接跑。'
tags: ["AI", "工具技巧", "故障诊断", "经验教训"]
categories: ["技术"]
draft: false
---

> 本来打算写一个协议转换代理，结果发现人家早就原生支持了。

---

## 起因

我想用 Claude Code——Anthropic 出的那个终端 AI 编程助手，挺有名的。但问题来了：它直连的是 Anthropic 官方 API，要海外信用卡，还得翻墙。

换 DeepSeek 呗。国内大厂，支付宝就能充值，还是 Anthropic 兼容协议——等一下，真兼容吗？

网上一搜，各种"搭代理"、"协议桥接"的教程满天飞。我差点就去写一个 HTTP 代理了 😅

---

## 多此一举的代理

别急着搭代理。先动手试一下：

```bash
curl https://api.deepseek.com/anthropic/v1/messages \
  -H "x-api-key: <你的key>" \
  -H "anthropic-version: 2023-06-01" \
  -d '{"model":"deepseek-v4-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":10}'
```

**直接就返回了完整的 Anthropic 格式响应。** 什么都不用转。

原来 DeepSeek 在 `/anthropic` 这个子路径下面，悄悄地搭了一个完整的 Anthropic Messages API 兼容层。Claude Code 发的请求格式，它原样吃进去、原样吐回来。

这就是第一个坑的来源：**配 base URL 的时候很容易把路径写错。**

---

## 配错地址，白折腾半小时

Claude Code 的配置里有一个 `ANTHROPIC_BASE_URL`，告诉它去哪找 API。很自然地，你会写成：

```
https://api.deepseek.com        ← 少了 /anthropic！
```

Claude Code 拿着这个地址，去请求 `/v1/messages`——也就是 `https://api.deepseek.com/v1/messages`。DeepSeek 的根路径下根本不知道 `/v1/messages` 是啥，直接扔了个 404。

正确的是：

```
https://api.deepseek.com/anthropic
```

这样请求才会打到 `/anthropic/v1/messages`，DeepSeek 就认了。

> 这个坑的隐蔽之处在于：不会报认证错误，不会报协议错误，就是 404。容易让人以为"DeepSeek 不支持 Anthropic 格式"而直接放弃了。

---

## 第二个坑：网上教程的模型名全过期了

配好了地址，模型名从哪来？随便翻一篇教程，上面写着：

```
deepseek-chat       # 轻量
deepseek-reasoner   # 推理
```

填进去——`model not found`。

DeepSeek 升级到 V4 后，模型名改成了：

```
deepseek-v4-flash   # 快速
deepseek-v4-pro     # 深度
```

而网上的教程、甚至 cc-switch 这类切换工具的内置预设，还在用旧名字。这种第三方预设不更新的问题在国内很常见，模型迭代太快了。

**怎么确认模型名还活着？** 直接调一下 DeepSeek 的 `/models` 端点，返回什么就用什么，不听教程的。

---

## 第三个坑：目录没建，写入失败

cc-switch 切换 provider 的时候，要去写 `~/.claude/settings.json`。但如果 Claude Code 是第一次装，`~/.claude/` 这个目录根本不存在，写入直接报错。

就一行的事：

```bash
mkdir ~/.claude
```

但没人告诉你，命令行也不会提示"请先创建目录"——它就报一个 `ENOENT`，自己去猜吧。

---

## 总结

| 坑 | 怎么绕过去 |
|----|-----------|
| 地址写成根路径 | 记得加 `/anthropic` 后缀 |
| 模型名过期 | 调 `/models` 端点确认，别信教程 |
| 目录不存在 | `mkdir ~/.claude` 先建好 |

这三个坑修完，配置长这样：

| 参数 | 值 |
|------|-----|
| `ANTHROPIC_BASE_URL` | `https://api.deepseek.com/anthropic` |
| Haiku 模型 | `deepseek-v4-flash` |
| Sonnet/Opus 模型 | `deepseek-v4-pro` |

没了。没有代理、没有 Docker、没有端口映射。一条命令直接跑。

---

## 回头想想

遇到"A 工具连 B 厂商"的问题，第一反应容易是"协议不兼容，要搭代理"。但很多大厂——DeepSeek、智谱、Kimi——其实都闷声提供了主流协议的兼容端点。

下次先发一个 `curl` 试一下，省下一下午写代理的时间。
