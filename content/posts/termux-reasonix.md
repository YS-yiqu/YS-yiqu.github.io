---
title: "在手机上装 AI 编程工具，我白折腾了一下午"
date: 2026-05-24T14:00:00+08:00
lastmod: 2026-06-08T18:01:00+08:00
description: '想在 Termux 里装个 AI 编程助手，结果掉进编译地狱。其实有 WASM 方案，2 分钟就搞定。'
tags: ["AI", "工具技巧", "故障诊断", "经验教训"]
categories: ["技术"]
draft: false
---

> 其实 2 分钟就能搞定，但我偏偏跟原生编译死磕了。

---

## 起因

我想在手机上跑一个终端 AI 编程助手——MIT 开源的，专为国内大模型优化，缓存命中率标称 99%。电脑上一条命令就装好了，手机上呢？有 Termux 终端嘛，应该也差不多对吧？`npm install -g` 走起——

**报错。**

然后就开始了跟编译错误搏斗的一个下午 😅

---

## 明明一条命令的事，为什么要绕这么大圈

问题的根儿在于：很多 npm 包的 `package.json` 里有个 `postinstall` 脚本，安装完成后会自动跑一些构建逻辑。作者写这些脚本的时候，默认你是 `npm install`（本地安装），而不是 `npm install -g`（全局安装）。

全局安装时当前目录不在包里，脚本里的路径全错——就像你在自己家厨房按别人家厨房的布局找盐罐子，肯定摸不着。

**最短路径是：不跟它死磕。直接 clone 源码自己装。**

```bash
git clone <仓库地址>
npm install --ignore-scripts   # 等会儿说为什么要加这个
npm run build
npm link                       # 效果 = 全局安装，命令照样到处能用
```

---

## 编译地狱：Python → NDK → make，一个比一个难搞

`npm install` 一跑，第一行就红了：

```
gyp ERR! find Python
```

哦对了，Node.js 的原生 C 扩展需要用 `node-gyp` 编译，而 `node-gyp` 是个 Python 脚本。Termux 默认没有 Python——装上就行，`pkg install python3`，简单。

再跑，又红了：

```
Undefined variable android_ndk_path
```

这次是 `tree-sitter-java` 这个包的编译脚本里写死了一个叫 `android_ndk_path` 的变量，在 Android 环境下必须手动传给它。Termux 有自己的 NDK 包，装上然后 export 一下：

```bash
pkg install ndk-multilib -y
export npm_config_android_ndk_path=$PREFIX/opt/ndk-multilib
```

好，再跑——**make 直接崩了。**

```
make: *** No rule to make target ... Stop.
```

到这里我已经折腾了快一小时。根本原因是：项目里有十几个 `tree-sitter-*` 包，每个包的编译脚本版本不一样，有的用旧版 gyp 语法，有的用新版，Termux 的 make 工具链兼容不过来。

**这时候我才发现自己一直在白忙。** 这工具根本不需要编译原生模块。

---

## 那个救命的 WASM

项目打包的时候，把 `tree-sitter-*.wasm` 都带上了。运行时检测到原生模块加载不了，自动 fallback 到 WASM——功能一模一样，速度没区别。

所以真正需要的安装命令是这样的：

```bash
npm install --ignore-scripts   # 跳过所有 postinstall，不碰 make
npm run build                  # 把 WASM 文件拷到运行目录
npm link                       # 注册全局命令
```

**`--ignore-scripts` 是关键。** 有了它，什么 Python、NDK、make 全都不用管。整个过程不超过两分钟。

---

## 最后一个坑：明明装好了，命令敲不出来

```
reasonix --version
→ command not found
```

三个可能：
- `npm link` 静默挂了 → 去看看 `npm config get prefix` 下的 bin 目录有没有这个命令
- build 没真正跑完 → 重新 `npm run build`
- Shell 还没刷新 → `hash -r` 一下

---

## 回头想想

如果一开始知道有 WASM 方案，我根本不会去装 Python 和 NDK。之所以会绕远路，是因为**习惯了"报错就修，修了再跑"的模式**，没有先停下来问一句：这项目是不是压根不需要编译？

| 教训 | 下次怎么做 |
|------|-----------|
| 报错先别急着修 | 先看项目文档有没有说"跳过编译" |
| 有 WASM 就别碰 make | 跨平台时代，原生编译是最后手段 |
| `--ignore-scripts` 是好东西 | 跳过自动脚本，手动构建更可控 |
| Termux 环境变量要手设 | 但不是每次都需要 |
