---
title: "PDF 签了名就不能高亮？原来是它在搞鬼"
date: 2026-06-09T09:00:00+08:00
lastmod: 2026-06-09T09:00:00+08:00
description: 'PDF 文字看得见但选不中？不是扫描件，是数字签名把文档锁了。断掉 AcroForm 和 Perms 引用，垃圾回收即可解锁。'
tags: ["故障诊断", "工具技巧", "AI", "经验教训"]
categories: ["技术"]
draft: false
---

你有没有遇到过这种情况：打开一个 PDF，明明文字看得清清楚楚，想用鼠标选中高亮，结果**死活选不了**？😤

最近就撞上了这件事——某国标 PDF，封面那页的 ICS 号想划个线都划不动。一查，好家伙，是数字签名在作祟。

---

## 现象

- PDF 在阅读器（Adobe Reader / Edge / Chrome）中**无法选中文字**
- 右键菜单里"高亮"、"下划线"等标注工具**灰色不可用**
- 但页面显示完全正常，不是扫描件

---

## 根因：数字签名锁了文档

这个 PDF 被签发工具（iText）打上了**数字签名**，而且签的是"认证签名"（Certification Signature），带了 `DocMDP`（Modification Detection and Prevention）标记。

PDF 阅读器的逻辑是：

> "这份文档被签了名，我不能让你在上面做任何修改——哪怕只是划个高亮，也是在 PDF 上加标注（annotation），算'修改'。为了签名有效性，我锁。"

签名的关键字段：
- `SigFlags = 3`：签名存在 + 需要验证
- `/DocMDP`：修改检测与防护
- `/SubFilter /adbe.pkcs7.detached`：PKCS#7 分离式签名

**本质**：这不是 PDF 权限（Permissions）的问题，而是阅读器对"已签名文档完整性"的保护策略。哪怕 PDF 权限位显示"允许注释"，阅读器也不让你标注。

---

## 解决思路

核心逻辑很简单：**把签名信息从 PDF 结构里摘掉。**

PDF 的数字签名信息存储在 PDF 的 Catalog 字典里，通过两个键引用：
- `/AcroForm` → 指向签名字段 → 指向签名数据
- `/Perms` → 指向 `DocMDP` 认证签名

只要把这两个引用断掉，再让 PDF 工具做一次垃圾回收（清除孤儿对象），签名就没了——文档内容毫发无损。

```python
# 核心操作（示意）
doc.xref_set_key(catalog, 'AcroForm', 'null')  # 断掉签名字段引用
doc.xref_set_key(catalog, 'Perms', 'null')      # 断掉认证签名引用
doc.save('output.pdf', garbage=4)               # 垃圾回收，清除孤儿签名对象
```

---

## 框架：签名 PDF 解锁通用方法

这个方法适用于任何"因为数字签名导致无法标注"的 PDF：

```
打开 PDF → 定位 Catalog → 删除 /AcroForm 和 /Perms 引用 → 垃圾回收 → 保存
```

**兜底方案**：如果上述方法不生效（某些 PDF 结构特殊），用"打印到 PDF"也能去签——Microsoft Print to PDF 或 Chrome 打印功能都能生成无签名副本。

---

## 备注

- 签名去掉后**高亮、划线、批注**都能正常使用
- 但如果原 PDF 的字体编码本身有问题（ToUnicode CMap 映射错误），复制出来的文字可能还是乱码——那是另一个问题，需要 OCR 处理
- 本方法不破坏页面内容、排版、图片，仅去掉签名锁
