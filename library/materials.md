---
title: 资料
aside: false
---

<script setup>
import PostList from "@/components/List/PostList.vue"
import Aside from "@/components/Aside/index.vue"

const mdModules = import.meta.glob('/pages/library/materials/*.md', { eager: true })
const txtGlob = import.meta.glob('/pages/library/materials/*.txt')
const pdfGlob = import.meta.glob('/pages/library/materials/*.pdf')

const files = [
  ...Object.entries(mdModules).map(([path, mod]) => {
    const fileName = path.split('/').pop().replace(/\.md$/, '')
    const fm = mod.frontmatter || {}
    return {
      regularPath: '/pages/library/materials/' + encodeURIComponent(fileName),
      title: fm.title || fileName,
      date: fm.date ? new Date(fm.date).getTime() : null,
      tags: fm.tags || [],
      description: fm.description || '',
      cover: fm.cover || null,
    }
  }),
  ...Object.keys(txtGlob).map((path) => {
    const fileName = path.split('/').pop().replace(/\.txt$/, '')
    return {
      regularPath: '/pages/library/materials/view/' + encodeURIComponent(fileName),
      title: fileName,
      date: null,
      tags: [],
      description: '',
      cover: null,
    }
  }),
  ...Object.keys(pdfGlob).map((path) => {
    const fileName = path.split('/').pop().replace(/\.pdf$/, '')
    return {
      regularPath: '/pages/library/materials/pdf/' + encodeURIComponent(fileName),
      title: fileName,
      date: null,
      tags: [],
      description: '',
      cover: null,
    }
  }),
].sort((a, b) => (b.date || 0) - (a.date || 0))
</script>

<div class="home-content">
  <div class="posts-content">
    <PostList :listData="files" />
  </div>
  <Aside />
</div>

<style scoped>
.home-content {
  width: 100%;
  display: flex;
  flex-direction: row;
}
.posts-content {
  width: calc(100% - 300px);
  transition: width 0.3s;
}
@media (max-width: 1200px) {
  .posts-content {
    width: 100%;
  }
}
</style>
