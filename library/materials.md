---
title: 资料
aside: false
---

<script setup>
import { useData } from "vitepress"
import LibraryList from "@/components/List/LibraryList.vue"

const { theme } = useData()
const allFiles = theme.value.materialsData || []
</script>

<LibraryList :data="allFiles" :show-categories="true" :show-tags="true" :show-aside="true" />
