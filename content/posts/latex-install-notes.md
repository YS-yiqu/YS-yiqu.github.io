---
title: "LaTeX 的学习记录——「安装」"
date: 2024-09-03
lastmod: 2024-09-03
description: "记录了 LaTeX 环境的安装过程，包括 Tex Live 安装、VSCode 配置和常见问题"
tags: ["LaTeX"]
categories: ["技术"]
draft: false
---

## 依据

- [LaTeX：从入门到日常使用](https://dylandong.top/posts/e480/)
- [VSCode 配置 Latex 环境](https://blog.csdn.net/qq_45952740/article/details/131004722)
- [配置 Visual Studio Code 和 LaTex 环境](https://yangyq.net/2022/05/latex-with-visual-studio-code.html)
- [resume-ng 模板库](https://github.com/fky2015/resume-ng)
- [LaTeX 模板库](https://www.latextemplates.com/)

## 学习目的

- 对 RELAP5 说明书技术文档的部分内容进行翻译，但是又不想花太多时间在格式上
- Word 的格式编辑蛮废时间的，Markdown 虽然不用在意排版，但是相对来说有些简陋，尤其对于翻译内容很少来说，就很奇怪
- 另一个目的是想要制作一个稍微好看点儿的简历，正反两面的，有英文有中文的
- 另一个想法是能够作为一个 PPT 的模板使用，想法很多，能够实现的内容较多

## 设立节点

- [x] 安装 Tex Live | 选择合适的简历模板 | 选择合适的技术报告模板
  **怎么没有人提及这个安装巨慢啊，大约半小时多了都**
- [x] VSCode 配置 Latex 环境
- [x] Hello World！
- [x] 更改简历信息，中文简历一份
- [x] 技术报告的雏形，填写翻译内容

## 十万个为什么？

1. beamer 是啥？
2. 下载链接失效，需要寻找新的镜像连接
3. TexLive2024 如何安装？exe 文件不是安装方式？是 bat 的形式安装吗？
4. Templates 是啥意思？
5. CV 是啥意思？
6. 安装完毕后，咋看是不是安装好了？
7. json 文件是啥意思？
8. 遇到兼容性错误
   ```
   LaTeX Workshop is incompatible with "vscode-pdf". We compete when opening a PDF file from the sidebar. Please consider disabling either extension.
   ```
9. 出现错误
   ```
   Recipe terminated with fatal error: spawn latexmk ENOENT.
   ```
10. LaTex 采用模板编译时，中文字符出现问题
11. 使用 VSCode 环境遇到问题太多了，一直有报错干扰，用专门的编译器试试看
12. VSCode 环境中遇到 latex 文件报错解决思路是啥？
13. tlmgr 是啥用途？

## 个人理解「答：」

1. [从零开始用 beamer 做学术报告幻灯片](https://alexander-qi.github.io/2019/teachbeamer/)，PPT 的另一种说法
2. [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/ctan/systems/texlive/Images/)
3. [最新 Latex2024 安装教程 超简单](https://blog.csdn.net/qq_43511299/article/details/137785036)
4. 模板的意思
5. Curriculum Vitae，个人履历，和简历还是有区别的
6. 标准方法，看版本号：`latex -v`
7. VSCode 的 launch.json 是设定规则来调试代码的，默认是不带这个规则的
8. 需要修改对应的 json，默认选项被锁定了，只能修改用户选项，复制后可以使用
9. 要注意 document class，有些不支持中文的
10. 使用推荐的 TeXworks，发现编译格式蛮重要的，还是得逐步去了解原理
11. 查看报错信息，利用 GPT 或者互联网查询错误，最后发现是一些库没有安装好/版本不够新，使用 tlmgr 把所有包都升级一遍，能够编译，凑合用，但是依旧有一堆警告，暂时先告一段落
