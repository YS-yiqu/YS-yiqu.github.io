---
title: "干活之前，先走这五步"
date: 2026-09-29T21:50:00+08:00
description: '接活第一件事不是上手，是问清目标和节点。先翻现成的文件，再跑现场，最后找人；方法自己定，成败判据对外对齐；先出个小的，三轮内定稿；干完记一笔。'
tags: ["方法论", "工作方法", "经验教训"]
categories: ["技术"]
draft: false
---

<style>
/* ===== 本篇专用配色（只在本页输出，不影响其他文章） ===== */
.post-single .post-content { counter-reset: fs-step; }

/* 大标题：蓝→橙 渐变字 */
@supports (-webkit-background-clip: text) {
  .post-single .post-title {
    background: linear-gradient(100deg, #10305a 0%, #2d8cf0 60%, #ff7a45 118%);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
}

/* 引言卡：淡蓝底 + 蓝色左条 */
.post-single .post-content > blockquote {
  margin: 26px 0 36px;
  padding: 15px 20px;
  background: linear-gradient(135deg, #f1f8ff 0%, #e9f4ff 100%);
  border: 1px solid #d3e7ff;
  border-left: 4px solid #2d8cf0;
  border-radius: 10px;
}
.post-single .post-content > blockquote p {
  margin: 0;
  color: #1c4f8a;
  font-weight: 500;
}

/* 章节标题：编号徽章 + 深蓝字 + 蓝橙渐变细线 */
.post-single .post-content > h2 {
  counter-increment: fs-step;
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 44px 0 16px;
  padding-bottom: 10px;
  font-size: 20px;
  font-weight: 600;
  color: #143a63;
  background-image: linear-gradient(90deg, #2d8cf0 0%, #8fd0ff 35%, #ffcbb0 70%, rgba(255, 203, 176, 0) 100%);
  background-repeat: no-repeat;
  background-position: 0 100%;
  background-size: 100% 1.5px;
}
.post-single .post-content > h2::before {
  content: counter(fs-step, decimal-leading-zero);
  flex: 0 0 auto;
  padding: 4px 7px 3px;
  font-family: Consolas, Menlo, monospace;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: .5px;
  color: #fff;
  border-radius: 6px;
  background: linear-gradient(135deg, #2d8cf0 0%, #55b8f5 100%);
  box-shadow: 0 2px 7px rgba(45, 140, 240, .30);
}

/* 正文强调 */
.post-single .post-content strong { color: #143a63; }

/* 收尾一句：暖橙卡片 */
.post-single .post-content > p:last-of-type {
  margin: 30px 0 6px;
  padding: 12px 16px;
  background: linear-gradient(135deg, #fff8f3 0%, #fff2e8 100%);
  border-left: 4px solid #ff7a45;
  border-radius: 0 10px 10px 0;
  color: #a9411c;
  font-weight: 600;
}
</style>

> 一件事干砸，多半不是手上不行，是动手之前没问清楚。

## 先把目标问清楚

接活第一件事，别急着上手，先找派活的人对齐两句话：做到什么程度、什么时候交。

这两句要是自己拍，后面大概率返工——你干得很漂亮，他要的是另一样东西。目标定了，后面心里才有底。

## 调研比灵感可靠

现场勘验也好，翻历史文件也好，都是调研。

先翻纸面。同类的事以前干过没有，有没有现成文件可以照着改。纸面不欠人情，而且写得比人说得全。

再上现场。实际条件长什么样，常常和纸上写的不一样，这一步省不得。

最后找人。前面两步都没解决的，才去问干过这事的人。问法也换一下：别问“我该怎么办”，问“你当时第一步做了啥”。他要回答前者，得替你想一遍；回答后者，只要说事实。

## 方法自己定，标准问别人

调研完，怎么干、谁干、什么时候干，这些自己拿主意，不必处处请示。

但一件事“算不算成”不能自己认定，得跟人对齐。方案被推翻，多半不是方案不好，是两边认定的成败标准不一样。

## 先出个小的

一次干完再交，方向错了就全白干。先出一页、一段、一次试跑，拿反馈再往下铺。

迭代最多三轮：第一轮对方向，第二轮对口径，第三轮定稿。每轮写清这轮改什么，改完就停。三轮之后还有分歧，问题通常不在稿子上，回头把目标重新对齐一遍。

## 干完记一笔

哪条判断错了、哪条漏了，记下来。

不记，只是干完一件事；记了，下次遇到同类的活，直接调出来用。

调研摸清情况，方法自己定，判据对外对齐。
