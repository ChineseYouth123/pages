---
title: 资料
aside: false
---

<script setup>
import { ref, computed, watch } from "vue"
import { useData } from "vitepress"
import PostList from "@/components/List/PostList.vue"
import Aside from "@/components/Aside/index.vue"

const { theme } = useData()
const postSize = theme.value.postSize || 10

// 数据来自全局配置（configuration approach）
const allFiles = theme.value.materialsData || []

// 分类列表
const allCategories = [...new Set(
  allFiles.flatMap((f) => f.categories),
)].sort((a, b) => a.localeCompare(b, "zh-CN"))

// 标签列表
const allTags = [...new Set(
  allFiles.flatMap((f) => f.tags),
)].sort((a, b) => a.localeCompare(b, "zh-CN"))

const selectedCategory = ref("")
const selectedTag = ref("")
const currentPage = ref(1)

const categoryCount = computed(() => {
  const counts = {}
  allFiles.forEach((f) => f.categories.forEach((c) => { counts[c] = (counts[c] || 0) + 1 }))
  return counts
})

const tagCount = computed(() => {
  const counts = {}
  allFiles.forEach((f) => f.tags.forEach((t) => { counts[t] = (counts[t] || 0) + 1 }))
  return counts
})

const filteredData = computed(() => {
  let data = allFiles
  if (selectedCategory.value) {
    data = data.filter((f) => f.categories.includes(selectedCategory.value))
  }
  if (selectedTag.value) {
    data = data.filter((f) => f.tags.includes(selectedTag.value))
  }
  return data
})

const totalPages = computed(() => Math.ceil(filteredData.value.length / postSize))

const pagedData = computed(() => {
  const start = (currentPage.value - 1) * postSize
  return filteredData.value.slice(start, start + postSize)
})

watch([selectedCategory, selectedTag], () => {
  currentPage.value = 1
})
</script>

<div class="materials">
  <div class="materials-content">
    <!-- 分类 -->
    <div v-if="allCategories.length" class="type-bar s-card hover">
      <div class="all-type">
        <a
          :class="['type-item', { choose: !selectedCategory }]"
          @click="selectedCategory = ''"
        >全部</a>
        <a
          v-for="cat in allCategories"
          :key="cat"
          :class="['type-item', { choose: selectedCategory === cat }]"
          @click="selectedCategory = cat"
        >{{ cat }} <span class="num">{{ categoryCount[cat] }}</span></a>
      </div>
    </div>
    <!-- 标签 -->
    <div v-if="allTags.length" class="type-bar s-card hover">
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
    <!-- 列表 -->
    <PostList :listData="pagedData" />
    <!-- 分页 -->
    <div v-if="totalPages > 1" class="pagination">
      <div
        :class="['page-item', 'prev', { disabled: currentPage <= 1 }]"
        @click="currentPage > 1 && currentPage--"
      >
        <i class="iconfont icon-page-right" />
        <span class="page-text">上页</span>
      </div>
      <div class="page-number">
        <div
          v-for="p in totalPages"
          :key="p"
          :class="['page-item', { choose: p === currentPage }]"
          @click="currentPage = p"
        >
          <span class="page-num">{{ p }}</span>
        </div>
      </div>
      <div
        :class="['page-item', 'next', { disabled: currentPage >= totalPages }]"
        @click="currentPage < totalPages && currentPage++"
      >
        <span class="page-text">下页</span>
        <i class="iconfont icon-page-right" />
      </div>
    </div>
  </div>
  <Aside />
</div>

<style scoped>
.materials {
  width: 100%;
  display: flex;
  flex-direction: row;
}
.materials-content {
  width: calc(100% - 300px);
  transition: width 0.3s;
}
@media (max-width: 1200px) {
  .materials-content {
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
}
.type-bar .all-type {
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
}
.type-bar .all-type .type-item {
  display: flex;
  align-items: center;
  padding: 0.1rem 0.5rem;
  margin-right: 6px;
  font-weight: bold;
  border-radius: 8px;
  white-space: nowrap;
  height: 30px;
  cursor: pointer;
}
.type-bar .all-type .type-item .num {
  margin-left: 4px;
  font-weight: normal;
  padding: 2px 6px;
  font-size: 0.75rem;
  color: var(--main-font-color);
  background-color: var(--main-card-border);
  border-radius: 8px;
}
.type-bar .all-type .type-item.choose {
  color: var(--main-card-background);
  background-color: var(--main-color);
}
.type-bar .all-type .type-item.choose .num {
  color: var(--main-color);
  background-color: rgba(255, 255, 255, 0.2);
}
.type-bar .all-type .type-item:hover {
  color: var(--main-card-background);
  background-color: var(--main-color);
}
.type-bar .all-type .type-item:hover .num {
  color: var(--main-color);
  background-color: rgba(255, 255, 255, 0.2);
}
.pagination {
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  width: 100%;
  height: 40px;
  animation: fade-up 0.6s 0.4s backwards;
}
.pagination .page-item {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  border-radius: 8px;
  overflow: hidden;
  background-color: var(--main-card-background);
  border: 1px solid var(--main-card-border);
  box-shadow: 0 8px 16px -4px var(--main-border-shadow);
  transition:
    border-color 0.3s,
    box-shadow 0.3s;
  cursor: pointer;
}
.pagination .page-item.prev,
.pagination .page-item.next {
  position: absolute;
  width: 80px;
}
.pagination .page-item.prev {
  left: 0;
}
.pagination .page-item.prev .iconfont {
  transform: rotate(180deg);
  transition:
    color 0.3s,
    transform 0.3s;
}
.pagination .page-item.prev .page-text {
  margin-left: 4px;
  margin-right: -36px;
}
.pagination .page-item.next {
  left: auto;
  right: 0;
}
.pagination .page-item.next .page-text {
  margin-right: 4px;
  margin-left: -36px;
}
.pagination .page-item .page-text {
  opacity: 0;
  transition:
    opacity 0.3s,
    margin 0.3s;
}
.pagination .page-item.disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
.pagination .page-item:hover {
  border-color: var(--main-color);
  box-shadow: 0 8px 16px -4px var(--main-color-bg);
}
.pagination .page-item:hover .iconfont {
  color: var(--main-color);
}
.pagination .page-item:hover .page-text {
  opacity: 1;
  margin-right: 0;
}
.pagination .page-item.next:hover .page-text {
  margin-right: 4px;
  margin-left: 0;
}
.pagination .page-item.choose {
  color: var(--main-card-background);
  border-color: var(--main-color);
  background-color: var(--main-color);
  box-shadow: 0 8px 16px -4px var(--main-color-bg);
}
.pagination .page-number {
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.pagination .page-number .page-item {
  margin: 0 6px;
}
@media (max-width: 768px) {
  .pagination .page-number {
    display: none;
  }
}
</style>
