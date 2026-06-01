---
title: 毛泽东
---

# 毛泽东

毛泽东相关电子图书资料。

<script setup>
import { useData } from "vitepress"
import LibraryList from "@/components/List/LibraryList.vue"

const { theme } = useData()
const allFiles = theme.value.maozedongData || []
</script>

<LibraryList :data="allFiles" :show-categories="false" :show-tags="false" :show-aside="false" />
