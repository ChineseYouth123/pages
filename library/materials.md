---
title: 资料
aside: false
---

<script setup>
import { ref, computed } from "vue"
import PostList from "@/components/List/PostList.vue"
import Aside from "@/components/Aside/index.vue"

const mdModules = import.meta.glob('/pages/library/materials/*.md', { eager: true })
const txtGlob = import.meta.glob('/pages/library/materials/*.txt')
const pdfGlob = import.meta.glob('/pages/library/materials/*.pdf')

const allFiles = [
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

const allTags = [...new Set(
  Object.entries(mdModules).flatMap(([, mod]) => mod.frontmatter?.tags || [])
)].sort((a, b) => a.localeCompare(b, 'zh-CN'))

const selectedTag = ref('')

const tagCount = computed(() => {
  const counts = {}
  allFiles.forEach(f => {
    f.tags.forEach(t => { counts[t] = (counts[t] || 0) + 1 })
  })
  return counts
})

const files = computed(() => {
  if (!selectedTag.value) return allFiles
  return allFiles.filter(f => f.tags.includes(selectedTag.value))
})
</script>

<div class="home-content">
  <div class="posts-content">
    <div class="type-bar s-card hover">
      <div class="all-type">
        <a
          :class="['type-item', { choose: !selectedTag }]"
          @click="selectedTag = ''"
        >全部</a>
        <a
          v-for="tag in allTags"
          :key="tag"
          :class="['type-item', { choose: selectedTag === tag }]"
          @click="selectedTag = tag"
        >{{ tag }} <span class="num">{{ tagCount[tag] }}</span></a>
      </div>
    </div>
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
.type-bar {
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 1rem;
  padding: 0.6rem;
  font-weight: bold;
  animation: fade-up 0.6s 0.3s backwards;
  .all-type {
    width: 100%;
    display: flex;
    flex-direction: row;
    align-items: center;
    overflow: hidden;
    mask: linear-gradient(
      90deg,
      #fff 0,
      #fff 90%,
      hsla(0, 0%, 100%, 0.6) 95%,
      hsla(0, 0%, 100%, 0) 100%
    );
    .type-item {
      display: flex;
      align-items: center;
      padding: 0.1rem 0.5rem;
      margin-right: 6px;
      font-weight: bold;
      border-radius: 8px;
      white-space: nowrap;
      height: 30px;
      cursor: pointer;
      .num {
        margin-left: 4px;
        font-weight: normal;
        padding: 2px 6px;
        font-size: 0.75rem;
        color: var(--main-font-color);
        background-color: var(--main-card-border);
        border-radius: 8px;
      }
      &.choose {
        color: var(--main-card-background);
        background-color: var(--main-color);
        .num {
          color: var(--main-color);
          background-color: rgba(255,255,255,0.2);
        }
      }
      &:hover {
        color: var(--main-card-background);
        background-color: var(--main-color);
        .num {
          color: var(--main-color);
          background-color: rgba(255,255,255,0.2);
        }
      }
    }
  }
}
</style>
