---
title: 信箱
---

# 📬 信箱

欢迎来到信箱，这里收录了各类来信与投稿资料。

## 分类导航

| 类别 | 说明 | 链接 |
|------|------|------|
| 所有文件 | 信箱全部资料 | [进入 →](./mailbox) |

<script setup>
const txtGlob = import.meta.glob('/pages/mailbox/*.txt')
const pdfGlob = import.meta.glob('/pages/mailbox/*.pdf')

const files = [
  ...Object.keys(txtGlob).map((path) => {
    const name = path.split('/').pop().replace(/\.txt$/, '')
    return { path, name, link: '/pages/mailbox/view/' + encodeURIComponent(name), ext: '.txt' }
  }),
  ...Object.keys(pdfGlob).map((path) => {
    const basename = path.split('/').pop()
    const name = basename.replace(/\.pdf$/, '')
    return { path, name, link: '/pages/mailbox/' + encodeURIComponent(basename), ext: '.pdf' }
  }),
].sort((a, b) => a.name.localeCompare(b.name, 'zh-CN'))
</script>

<FileList v-if="files.length" :files="files" />
