---
title: "LaTeX 从安装到真香：一个初学者的踩坑全记录"
date: 2024-09-03T16:00:00+08:00
lastmod: 2024-09-03T16:00:00+08:00
description: "Tex Live 安装要半小时？VSCode 配 LaTeX 配置到崩溃？从零开始折腾 LaTeX 的全过程踩坑记录。"
tags: ["LaTeX", "工具技巧"]
categories: ["技术"]
draft: false
---

LaTeX 是一个专业排版系统，学术界写论文、做简历、做 PPT 都很强。但它的入门门槛也确实高——光安装就能劝退不少人。

这篇文章记录了我从安装到跑通第一个文档的全部过程。

---

## 为什么学 LaTeX？

我的实际需求：

| 需求 | 为什么选 LaTeX |
|------|---------------|
| 翻译技术文档 | Word 排版太费时间，LaTeX 自动处理格式 |
| 做简历 | LaTeX 模板做出来的简历比 Word 精致 |
| 做 PPT | LaTeX 的 beamer 可以做出学术风格的演示 |

总之，**不想花时间调格式**，就是学 LaTeX 最大的动机。

---

## 安装清单

需要装两个东西：

### 1. Tex Live（核心引擎）

从 [清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/ctan/systems/texlive/Images/) 下载，比官网快很多。

> ⚠️ **注意**：安装非常慢，大概需要 **半小时以上**，要有心理准备。

安装完后验证：

```bash
latex -v
```

看到版本信息就说明装好了。

### 2. VSCode + LaTeX Workshop 插件

在 VSCode 扩展商店搜索 `LaTeX Workshop` 安装即可。

---

## 踩坑记录

### 坑1：LaTeX Workshop 插件冲突

**现象**：安装后提示 `LaTeX Workshop is incompatible with "vscode-pdf"`

**解决**：禁用或卸载 `vscode-pdf` 插件，两个功能冲突。

### 坑2：编译报错找不到 latexmk

**现象**：点编译就报错，说找不到 latexmk。

**原因**：VSCode 的 LaTeX 配置文件里默认调用了某个工具，但系统没装。

**解决**：修改 VSCode 的用户设置（`settings.json`），把编译工具换成 `xelatex`。如果默认选项被锁定，复制到用户设置里再改。

### 坑3：中文字符不显示

**原因**：LaTeX 的 `documentclass` 默认不支持中文。

**解决**：使用 `ctexart` 或 `ctexbook` 文档类，或者用 `\usepackage[UTF8]{ctex}` 包。

### 坑4：各种莫名其妙的编译报错

**排查思路**：

1. 先看报错信息 —— 它通常会告诉你缺哪个包
2. 用 `tlmgr update --all` 升级所有宏包
3. 如果还不行，用 GPT 或搜索引擎查具体报错信息

大部分问题都是**版本不够新**或者**某个包没装全**导致的。

---

## 常用命令速查

| 命令 | 用途 |
|------|------|
| `latex -v` | 查看 LaTeX 版本 |
| `tlmgr update --all` | 升级所有宏包 |
| `tlmgr install <包名>` | 安装指定宏包 |

---

## 概念解释

| 术语 | 解释 |
|------|------|
| beamer | LaTeX 做 PPT 的方式 |
| CV | Curriculum Vitae，学术简历 |
| Templates | 模板，别人写好的格式你直接用 |
| tlmgr | TeX Live 的包管理器（类似 npm） |

---

## 经验总结

1. **安装用镜像**：官网下载等到天荒地老，清华镜像快得多
2. **中文用 ctex**：不要指望默认的 `article` 类能支持中文
3. **先本地跑通**：用简单的 "Hello World" 验证环境正常，再搞复杂的文档
4. **报错别慌**：LaTeX 的错误信息虽然长，但通常很准确。先看最后几行，再用搜索引擎

LaTeX 的第一关"安装"就这么通过了。虽然过程折腾，但配置好之后就舒服了。后面写文档，只需要关心内容，不用再跟 Word 的格式较劲 💪
