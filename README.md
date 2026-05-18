# 📄 pages — 站点内容

> 马列毛思想学习与交流站点的文档内容仓库。
>
> 作为 [chinese-youth-notes](https://github.com/ChineseYouth123/chinese-youth-notes) 的 Git 子模块使用。

## 目录结构

```
pages/
├── posts/                      # 📝 文章/学习笔记
│   └── 2026/                   #    按年归档
│       └── 2026-05-17.md
│
├── library/                    # 📚 文献库
│   ├── maozedong/              #   毛泽东著作
│   │   ├── pdf/                #     PDF 动态路由
│   │   └── view/               #     阅读页动态路由
│   ├── materials/              #   学习资料（历史文献、理论文章等）
│   │   ├── pdf/                #     PDF 动态路由
│   │   └── view/               #     阅读页动态路由
│   ├── red-books/              #   红书（红旗杂志等 PDF）
│   │   └── pdf/                #     PDF 动态路由
│   └── view/                   #   统一阅读页动态路由
│
├── categories/                 # 🏷️ 分类页面（动态路由）
│   ├── [name].md
│   └── [name].paths.mjs
│
├── tags/                       # 🔖 标签页面（动态路由）
│   ├── [name].md
│   └── [name].paths.mjs
│
├── 模板/                       # 📋 Markdown 编写模板示例
│
├── index.md                    # 🏠 首页
├── about.md                    # ℹ️  关于
├── archives.md                 # 📦 归档
├── categories.md               # 🏷️ 分类总览
├── tags.md                     # 🔖 标签总览
├── cc.md                       # 📄 声明/版权
├── thanks.md                   # 🙏 致谢
├── fankui.md                   # 💬 反馈
└── ai提示词.md                 # 🤖 AI 提示词
```

## 写作规范

### 文章（posts/）

文件名格式：`YYYY-MM-DD.md`，frontmatter 示例：

```yaml
---
title: 文章标题
date: 2026-05-17
tags:
  - 标签1
  - 标签2
categories:
  - 分类名
top: false
cover: /images/cover.jpg
description: 文章简介
---
```

### 文献（library/）

文献支持三种模式：
- **Markdown 文件** — 直接放在对应目录下
- **PDF 文件** — 放在 `*/pdf/` 目录，自动生成阅读页
- **阅读视图** — 通过 `*/view/` 动态路由提供

## 开发

```bash
# 在子模块内编辑
cd pages
git add . && git commit -m "描述修改"
git push

# 回到主仓库更新子模块引用
cd ..
git add pages && git commit -m "chore: update pages"
git push
```
