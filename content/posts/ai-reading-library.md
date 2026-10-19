---
title: "如何利用AI搭建线上阅读库"
date: 2026-10-19T17:10:00+08:00
lastmod: 2026-10-19T17:10:00+08:00
description: "把看过的影视、听过的专辑记下来，留下自己的意见。不是看完就算，起码留下点什么。"
tags: ["AI", "Hugo", "阅读库", "影音墙", "专辑墙"]
categories: ["技术"]
draft: false
---

<style>
.ai-tech { margin: 30px 0; border: 1px solid #e3e8ef; border-radius: 10px; padding: 14px 18px; background: #fafcff; }
.ai-tech summary { cursor: pointer; font-weight: 600; color: #2d8cf0; }
.ai-tech-actions { margin: 12px 0 4px; }
.ai-copy {
  border: 0; border-radius: 6px; padding: 7px 14px;
  background: #2d8cf0; color: #fff; font-size: 13px;
  cursor: pointer; font-family: inherit;
}
.ai-copy:hover { background: #1a73d1; }
.ai-tech-body { margin-top: 10px; font-size: 14px; line-height: 1.85; color: #333; }
.ai-tech-body h3 {
  margin: 20px 0 8px; padding-left: 10px;
  font-size: 16px; font-weight: 600; color: #1c4f8a;
  border-left: 3px solid #8fd0ff; border-radius: 2px;
}
.ai-tech-body ul { padding-left: 1.2em; }
.ai-tech-body li { margin: 4px 0; }
.ai-tech-body pre {
  margin: 10px 0; padding: 12px 14px;
  background: #f4f7fb; border: 1px solid #e3e8ef; border-radius: 6px;
  font-size: 13px; line-height: 1.7;
  white-space: pre-wrap; word-break: break-word;
}
</style>

看完一部片子，过几个月只记得“看过”，不记得当时为什么喜欢、哪一段被打动。豆瓣标记只能证明看过，不能满足“留下点什么”。

于是我用 AI 搭了一个线上阅读库。它有两面墙：一面是影视，一面是专辑。海报和封面只是索引，真正留下来的是评分、短评、曲目，以及点进去之后的那几句自己的话。

这里的“阅读”不局限于书。看过的电影、追过的剧、反复听的专辑，都可以像书一样归档；看完留一句，才算真的读过。

## 看完就忘，才是问题

我试过在豆瓣标记“看过”，也试过在备忘录里写两句。前者太轻，点一下就算完成；后者太散，写完就找不到了。时间一长，只记得“我看过”，不记得“我当时怎么想”。

我想要的是一个能回头翻的地方：有封面，有年份，有评分，最重要的是有一句自己的话。它不用像影评，哪怕只是“结尾那场雨下得好”，也比一个冷冰冰的“看过”有用。

## 我要的不是收藏夹，是一份带意见的记录

想清楚这一点，后面的取舍就简单了。

墙上的条目不是“想看”，是“看过”；不是把海报存下来，是看完之后愿意写一句。影音墙鼠标停上去，除片名和年份，还能看到我写在豆瓣的短评原文；点海报去豆瓣，是因为评分和影评的根在那里，站内不重复维护一份。

如果只存海报和评分，这面墙和豆瓣的“看过”列表没有区别。真正属于我的，是那一句短评。它不用长，但得有。

这也是 AI 最容易帮倒忙的地方：它能把墙做得很好看，但“写什么”只能自己来。AI 可以帮你取数、排版、调样式，不能替你回忆当时为什么被打动。

## AI 把脏活累活接走了

这次的分工很清楚。

AI 负责查、读、改：读 Jellyfin 的片库，整理 CSV；查豆瓣的评分和简介，取网易云的封面和歌曲 id；写 Hugo 模板，调悬停和网格；排错、构建、推送。重复劳动基本都交出去了。

人负责定、核、签：定“什么算看过”、选哪些条目上墙、写自己的短评、看截图验收。数据对不对、评分有没有抄错、封面比例是不是歪的，AI 不会替你判断；它只会把活干得很快。

我的用法是：把流程拆成能重复跑的步骤，让 AI 一段一段做；每一步都留一个能检查的结果。这样它出错时，能马上定位到是哪一段，而不是对着一个“看起来不对”的页面猜。

## 现在这个库长什么样

影音墙是一面海报墙，四列，鼠标停上去是 90% 黑遮罩，白字显示片名、年份、评分和我的短评；点海报去豆瓣。超过 20 张先出 20 张，点“加载更多”再出 20 张。

[![影音墙：鼠标停在《欢迎来龙餐馆》上，看到的是我写在豆瓣的短评](/images/ai-reading-library/media-wall.jpg)](/images/ai-reading-library/media-wall.jpg)

专辑墙是一面方形封面墙，悬停同样出遮罩，显示专辑名、艺人/年份、五星评分和曲目数；点封面进详情，里面有评分、曲目表、可折叠的专辑介绍，还有“播放专辑”按钮。点一下，播放器会临时建一个“专辑：呼吸之野”的列表，按顺序播；原来的歌单还在，切回去就恢复。

[![专辑墙：四列方形封面，悬停看评分和曲目数](/images/ai-reading-library/album-wall.jpg)](/images/ai-reading-library/album-wall.jpg)

[![专辑详情：评分、曲目表、专辑介绍和播放按钮](/images/ai-reading-library/album-detail.jpg)](/images/ai-reading-library/album-detail.jpg)

[![点“播放专辑”后，播放器里临时多一个专辑列表](/images/ai-reading-library/play-album.jpg)](/images/ai-reading-library/play-album.jpg)

两面墙的样式都只作用于本页，选择器带前缀，不碰主题全局。改一面墙，不会动到另一面。

## 做下来最值钱的几条经验

- **海报是索引，短评才是内容。** 没有自己的话，墙再好看也只是收藏夹。
- **数据和展示分开。** 加一部片、一张专辑，只动数据文件，不动模板。这样 AI 每次只需要改一个地方。
- **先做一张样张。** 先上一部片、一张专辑，确认版式、字段、交互，再全量铺；视觉类改动先看截图再上线。
- **AI 不会替你验收。** 本地构建成功不等于线上生效；数据、评分、封面比例、手机布局，得自己看一遍。

## 实现路径

先把总图放这儿：数据从哪来、落到哪、谁渲染、怎么上线。

[![线上阅读库实现路径](/images/ai-reading-library/flow.png)](/images/ai-reading-library/flow.png)

图有点宽，手机上点开看大图。上半部分是数据到线上的主流程，下半部分是详情页的播放支路。

拆成五步：

1. **取数。** 影视从 Jellyfin 读，专辑从豆瓣和网易云读。影视的评分、短评、豆瓣链接是手写列，同步时不能覆盖。
2. **落盘。** 影视写一份 CSV + 海报；专辑一个文件夹，`index.md` + `cover.jpg`。数据文件是唯一的事实来源，模板只读。
3. **渲染。** Hugo 用列表模板出墙，用详情模板出曲目页；样式只写在页面模板里，用 `.mw-` / `.aw-` 前缀。
4. **构建。** `hugo --minify` 生成 `public/`；本地先构建、预览、截图验收。
5. **推送与验收。** 只推改动的文件；CI 的 Deploy 和 Pages 都 success 后，再抓线上 HTML 和图片状态码。

播放是详情页的一条支路：点“播放专辑” → 按 `netease_id` 生成歌曲 → 塞进 APlayer 临时列表 → 按顺序播。原来的歌单不动，切回去恢复原设置。

伪代码：

```text
# 1. 取数
films  = jellyfin.movies_and_tv()
albums = read_album_list()            # slug / douban_id / netease_id

# 2. 落盘
write_csv("assets/movies.csv",
          merge(films, hand_written))  # 自动列覆盖，手写列保留
for a in albums:
    meta   = douban.album(a.douban_id)
    tracks = netease.album(a.netease_id)
    cover  = download(meta.cover)
    write_page("content/album/" + a.slug, meta, tracks, cover)

# 3. 渲染
hugo --minify                          # 数据 + 模板 → public/

# 4. 推送与验收
changed = diff(local_files, remote_tree)
push(changed)                          # 只推清单里的路径
wait_ci(); verify_live()               # Deploy + Pages + 线上 200

# 5. 播放
on "播放专辑":
    songs = album.tracks.map(t => meting_url(t.netease_id))
    player.replace(songs)
    player.play(0)                     # 按曲目顺序
```

## 技术细节

下面这段是给想自己搭一面墙的人准备的，也可以直接复制给 AI，让它按你的博客框架改。点“展开”看，点“复制”拿走。

<details class="ai-tech">
<summary>技术细节：数据和模板怎么搭（展开）</summary>
<div class="ai-tech-actions"><button class="ai-copy" type="button" data-copy="ai-tech-body">复制技术细节（喂给 AI）</button></div>
<div class="ai-tech-body" id="ai-tech-body">
<p><strong>下面是一份可复制的搭建说明。</strong>你可以整段发给 AI，让它按你的博客框架改；也可以自己照着做。前提是你已经有一个能构建的 Hugo 站点，影音墙放在哪个 section、专辑墙放在哪个路径，按你的目录改。</p>

<h3>一条流水线，四个固定件</h3>
<p>数据源 → 标准化数据 → 页面/模板 → 构建 → 推送 → 线上验收。每一步都能单独重跑，不要写成一个大脚本。</p>
<ul>
<li><strong>数据：</strong>影视用 CSV，一个条目一行；专辑用一个文件夹，<code>index.md</code> + <code>cover.jpg</code>。</li>
<li><strong>列表页：</strong>墙本身，四列网格 + 悬停信息 + 点击去处。</li>
<li><strong>详情页：</strong>可选。有曲目、长评、播放就做，没有就外链。</li>
<li><strong>样式：</strong>只作用于本页，选择器统一前缀，不碰主题全局。</li>
</ul>

<h3>数据怎么放</h3>
<p>影音墙：<code>assets/movies.csv</code>，列可以按自己的需要增减。我用的是 <code>title,year,rating,rating_source,quote,poster,douban,jellyfin_id,watched</code>。片名、年份、海报、id 是自动列，每次同步覆盖；评分、短评、豆瓣链接是手写列，同步时按 id 或片名匹配后保留。海报统一 2:3，缺图用同高虚线卡，别让网格跳。</p>
<p>专辑墙：<code>content/album/{slug}/index.md</code> + <code>cover.jpg</code>。front matter 至少放 <code>title</code>、<code>artist</code>、<code>year</code>、<code>rating</code>、<code>rating_count</code>、<code>douban</code>、<code>netease</code>、<code>tracks</code>；每首曲目带 <code>title</code>、<code>duration</code>、<code>netease_id</code>。封面统一 1:1，下载后缩到 700×700 左右。</p>
<p>CSV 放 <code>assets/</code>，不要放 <code>static/</code>；文件 UTF-8 无 BOM，字段含英文逗号要加引号，每行列数必须和表头一致。</p>

<h3>页面怎么写</h3>
<p>影音墙：四列，海报 <code>aspect-ratio: 2 / 3</code>、<code>object-fit: cover</code>，悬停 90% 黑遮罩、白字居中，0.3 秒淡入；超过 20 张先出 20 张，点“加载更多”再出 20 张；点海报去豆瓣。</p>
<p>专辑墙：四列，封面 <code>aspect-ratio: 1 / 1</code>，悬停同一套遮罩，显示专辑名、艺人/年份、五星评分、曲目数；点封面进详情。详情页放封面、艺人、发行日期、厂牌、流派、10 分制评分（五星含半星、评分人数）、曲目表、可折叠介绍，底部放“在网易云听”“豆瓣条目”“播放专辑”。</p>
<p>样式写在页面模板的 <code>&lt;style&gt;</code> 里，用 <code>.mw-</code>、<code>.aw-</code> 这类前缀；不要改 <code>custom.css</code>。触屏没有 hover，补 <code>:active</code> 和 <code>:focus-within</code>。</p>
<p>如果站点首页会列出所有 section，记得把 <code>mainSections = ["posts"]</code> 写死，否则墙上的条目一多，首页会被墙顶掉。</p>

<h3>播放怎么接</h3>
<p>不要另起播放器，复用站点已有的 APlayer。点“播放专辑”时：清空当前列表，按专辑曲目生成歌曲对象，<code>url</code> 用 <code>type=url&amp;id={netease_id}&amp;br=999</code>，封面用专辑封面；把列表塞进 APlayer，选中一个临时选项，播第一首。</p>
<p>两个细节：专辑按曲目顺序播，切回普通歌单要恢复原来的随机/顺序设置；切换歌单是异步的，旧请求晚到会覆盖新列表，用一个代次号把过期结果丢掉。</p>
<p>没有后端。歌曲 id 是播放的关键；如果某首因会员或下架取不到，给播放器加一张兜底源表，别让整张专辑卡死。</p>

<h3>部署怎么推</h3>
<p>本地构建、预览、截图验收；推送只推改动的文件。没有 git 的本地副本，可以用 GitHub API 拿远端 main 的 tree 做 <code>base_tree</code>，只覆盖清单里的路径，避免把别人的改动冲掉。推完看两条 CI（构建 + Pages 发布）都 success，再抓线上 HTML、封面状态码；本地构建成功不等于线上生效。</p>
<p>Jellyfin 同步只能在本机跑，GitHub 的构建机进不了家里的内网；只把 CSV 和海报推上去。</p>
<p>Jellyfin 只读账号、API key、推送 token 不要写进公开仓库，文章里也不要写 NAS 地址。</p>

<h3>容易踩的坑</h3>
<ul>
<li>豆瓣搜索、网易云接口会限流：降低频率、分批取、把结果落盘缓存，失败就跳过。</li>
<li>Hugo 的 <code>resources.Get</code> 读 CSV 返回二维数组，不是自动转好的 map，要按表头自己拼。</li>
<li>JSON 放进 <code>&lt;script&gt;</code> 要写 <code>jsonify | safeJS</code>，否则会被再转义一次，前端拿到的还是字符串。</li>
<li>图片比例和缺图占位要先定，否则网格高度会跳。</li>
<li>推送前先对账，只覆盖清单里的文件；推完必须看 CI 和线上。</li>
</ul>

<h3>可以直接发给 AI 的话</h3>
<pre>我在用 Hugo + PaperMod 搭一个线上阅读库，想加两面墙。请先给我最小样张，确认后再全量。

影音墙：读 assets/movies.csv，四列海报，2:3，悬停 90% 黑遮罩显示片名/年份/评分/短评，点海报去豆瓣；超过 20 张加载更多。
专辑墙：content/album/{slug}/index.md + cover.jpg，四列方形封面，悬停显示专辑名/艺人/年份/五星评分/曲目数，点封面进详情；详情页有封面、艺人、发行日期、厂牌、流派、10 分制评分、曲目表、可折叠介绍、在网易云听/豆瓣条目/播放专辑。

约束：数据与展示分离；样式只写在页面模板里，用前缀，不改全局主题；先做样张；构建后截图验收；不要编造评分。
数据源：影视来自 Jellyfin，专辑评分/简介来自豆瓣，封面/歌曲 id 来自网易云。</pre>
</div>
</details>

<script>
(function () {
  var btn = document.querySelector(".ai-copy");
  if (!btn || btn.getAttribute("data-bound") === "1") return;
  btn.setAttribute("data-bound", "1");
  btn.addEventListener("click", function () {
    var el = document.getElementById(btn.getAttribute("data-copy"));
    if (!el) return;
    var text = el.innerText || el.textContent || "";
    var old = btn.textContent;
    function done() {
      btn.textContent = "已复制";
      setTimeout(function () { btn.textContent = old; }, 1800);
    }
    function fallback() {
      var ta = document.createElement("textarea");
      ta.value = text;
      document.body.appendChild(ta);
      ta.select();
      try { document.execCommand("copy"); done(); } catch (e) {}
      document.body.removeChild(ta);
    }
    if (navigator.clipboard && navigator.clipboard.writeText) {
      navigator.clipboard.writeText(text).then(done).catch(fallback);
    } else {
      fallback();
    }
  });
})();
</script>

## 留下点什么

墙不是目的。看过就算了，还是留下点什么——这个库真正的价值，不在页面多好看，在每次看完之后愿意写下的那几句话。
