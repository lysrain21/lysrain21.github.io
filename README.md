# Lucian's Blog

一个基于 Jekyll 和 Minima 结构的极简个人博客，用于记录：

1. 会赚钱的产品拆解；
2. 每月一元投资练习；
3. 认知迭代。

## 本地运行

```bash
bundle install
bundle exec jekyll serve --baseurl ""
```

打开 `http://127.0.0.1:4000`。

## 添加文章

在 `_posts/` 新建 Markdown 文件。文件名格式为 `YYYY-MM-DD-title.md`。

产品拆解：

```yaml
---
layout: post
title: 文章标题
date: 2026-08-01 09:00:00 +0800
category: product
description: 首页使用的一句话摘要。
---
```

认知迭代只需将 `category` 改为 `cognition`。

## 更新投资练习

编辑 `_data/investments.yml`。每月为每家公司追加一条记录：

```yaml
- date: 2026-08-01
  price_cny: 108.50
```

`price_cny` 表示当日每股价格的人民币等值。页面会自动计算累计份额、当前市值和收益率。投资页不会出现在顶部导航中，入口位于页尾 GitHub 链接右侧。

## 部署

推送到 `main` 后，`.github/workflows/pages.yml` 会自动构建并部署 GitHub Pages。

当前博客地址是：

`https://lysrain21.github.io/`
